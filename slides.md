---
# You can also start simply with 'default'
theme: seriph
title: Linux Internals — Real-Time Linux
background:
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: none
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
---

# Real-Time Linux

Linux Internals Proseminar

Daniel Dietzler

<figure class="absolute flex flex-col items-center right-16 bottom-14">
  <img src="/qr-code.svg" class="w-40"/>
  <figcaption><a href="https://t1p.de/rt-linux">https://t1p.de/rt-linux</a></figcaption>
</figure>

---
layout: quote
---

### "Real-Time is not as fast as possible; it is as fast as specified."

_— Dr. Doug Niehaus, RT Linux team (RIP since Aug 30th, 2012)_


---

# Requirements of RTOS

<v-clicks>

- "Temporal determinism"
- Event driven
- Preemptible Kernel
- Priority-based process switching
- Verifiable/Provable

</v-clicks>

---

# Requirements on RT Linux

<div class="flex justify-between"> 

<v-clicks>

- Isolated (best effort)
- "Simple"
- Non-blocking
- Easy to use

</v-clicks>

<figure class="flex flex-col h-100">
  <img src="/LKM.svg" class="h-full"/>
  <figcaption class="ml-auto mr-auto"><a href="https://github.com/makelinux/linux_kernel_map" class="text-xs">https://github.com/makelinux/linux_kernel_map</a></figcaption>
</figure>

</div>

---

# Variants of Real-Time

<v-clicks depth=2>

- Soft RT
  - Degradation of QoS
  - Video streaming, Tidal, ...
- Firm RT
  - Too late? Result of no value
  - Trading, auctions (ebay), ...
- Hard RT
  - Never. Miss. A. Deadline. (Ever.)
  - Cars, airplanes, rockets, ...

</v-clicks>

<img v-click="3" src="/twitch.svg" class="w-20 self-end mr-90 -mt-76"/>
<img v-click="3" src="/teams.svg" class="w-20 self-end mr-20 -mt-10"/>
<img v-click="6" src="/stonks.png" class="w-36 self-end mr-46 mt-4"/>
<twemoji-rocket v-click="9" class="text-[3.5em] self-end mr-100 mt-6"/>

---

# Current State of RTOS

<v-clicks>

- Closed source, proprietary
- Expensive (licenses + devs)
- Legacy, unmaintained
- Specialized
- Barebones

</v-clicks>

---

#  RT Linux History: Chaos

<v-clicks depth=2>

- <2000: early research 
- Cokernel \[2]
  - Timing issues
  - Expensive memory swapping
  - Invasive
- 2004: uncoordinated efforts towards Kernel patches
  - $\Rightarrow$ Chaos

</v-clicks>

<figure class="w-86 flex flex-col items-end self-end -mt-60 -mb-6" v-click="[2, 6]">
  <img src="/cokernel.png" />
  <figcaption>[2]</figcaption> 
</figure>

<footer class="text-[9px] mt-auto font-thin">

\[0] https://www.linux.com/news/in-the-trenches-with-thomas-gleixner-real-time-linux-kernel-patch-set \
\[1] A. Casimiro, P. Martins and P. Verissimo, "How to build a timely computing base using real-time Linux," 2000 IEEE International Workshop on Factory Communication Systems. Proceedings (Cat. No.00TH8531), Porto, Portugal, 2000, pp. 127-134, doi: 10.1109/WFCS.2000.882542. \
\[2] Federico Reghenzani, Giuseppe Massari, and William Fornaciari. 2019. The Real-Time Linux Kernel: A Survey on PREEMPT_RT. ACM Comput. Surv. 52, 1, Article 18 (February 2019), 36 pages. https://doi.org/10.1145/3297714
</footer>

---

#  RT Linux History: `PREEMPT_RT`

<v-clicks depth=2>

- Around 2004, Kernel 2.4: Ingo Molnar (RedHat); `PREEMPT_RT`
- Loose team (with Dr. Doug Niehaus)
- Proof of Concept!
- Planned `PREEMPT_RT` release: October 2018
  - Well...

</v-clicks>

<footer class="text-[10px] mt-auto font-thin">

\[0] https://www.linux.com/news/in-the-trenches-with-thomas-gleixner-real-time-linux-kernel-patch-set
</footer>

---

# Digging Deeper: `PREEMPT_RT`

<v-clicks>

- Scheduler improvements
- Kernel analysis tools
- Latency/Timing optimizations
- Amount of Non-preemptible Kernel code 📉
- `rt_mutex` with priority inheritance
- Two new preemption levels

</v-clicks>

<footer class="text-[10px] mt-auto font-thin">

\[5] https://wiki.linuxfoundation.org/realtime/documentation/technical_details/start
</footer>

<!--
Latency/Timing optimizations actually surfaced bugs in the kernel \
priority inheritance to avoid priority inversion
-->

---

# Preemption Models

<v-clicks>

- No forced preemption (server)
- Voluntary Kernel Preemption (desktop)
- Preemptible Kernel (low-latency desktop)

</v-clicks>

<v-click>

New in `PREEMPT_RT`:

</v-click>

<v-clicks>

- Preemptible Kernel (basic RT)
- Fully Preemptible Kernel (RT)

</v-clicks>

<footer class="text-[10px] mt-auto font-thin">

\[6] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/preemption_models
</footer>

<!--
[click] Only syscalls and interrupts preempt \
[click] ^+ a few preemption points \
[click] NOT PREEEMPT_RT; all kernel code not in critical sections preemptible \
[click][click] ^ + forced threaded input handlers; mostly for testing \
[click] RT Linux! ^ + some critical sections preemptible
-->

---

# `printk`


<v-clicks>

- (Very) High latencies
- General rework long overdue
- Requires Kernel locks

</v-clicks>

<v-click>

Solution:
</v-click>

<v-clicks>

- Main idea: per-console locks
- Console priorities: `normal`, `emergency`, `panic`
- New kthreads for flushing `normal` messages
  - on NBCON consoles
  - on all consoles if `CONFIG_RT` enabled

</v-clicks>

<v-click at="7">

- ...

</v-click>

<footer class="text-[10px] mt-auto font-thin">

\[9] https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=c903327d3295b135eb8c81ebe0b68c1837718eb8
</footer>

<!--
NBCON; Non-blocking console, outside legacy console_lock constraints
-->

---

# `printk`: Linus Rant

<v-click>

- *"It's also NOT SANE."*
- *"End result: no way will I accept this kind of completely arbitrary and frankly not very intelligent patch."*
- *"Not this kind of idiotic tying together of things."*
- *"make it really clear how disappointed I am with the printk tree lately"*
- *"not only am I not pulling this, I'm concerned that I will not be pulling printk patches in the future either because of where these pull requests seem to be trending."*

</v-click>

<footer class="text-[10px] mt-auto font-thin">

\[8] https://lwn.net/ml/linux-kernel/CAHk-%3Dwie%2BVC-R5%3DHm%3DVrg5PLrJxb1XiV67Efx-9Cr1fBKCWHTQ%40mail.gmail.com
</footer>


---

# Current State of RT Linux

<v-clicks>

- `PREEMPT_RT` in Kernel 6.12; November 18th, 2024
- Linux Foundation (LF) ELISA
  - ARM, BMW, RedHat, Intel, Texas Instruments, ...
- `SCHED_DEADLINE`
- `RT_THROTTLING`
- `ARM64`, `x86`, `RISC-V`(!)

</v-clicks>

<footer class="text-[10px] mt-auto font-thin">

\[3] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/sched_policy_prio/sched_deadline \
\[4] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/sched_rt_throttling
</footer>

<!--
RT_THROTTLING: starvation due to high priority process running e.g. infinite loop

`RT_THROTTLING` isn't new, but problem is of importance with RT Linux
-->

---

# Complexities

<v-clicks depth=2>

- General Purpose OS (GPOS) $\perp$ RTOS
  - Execution time vs average performance
- (Multi core) task scheduling
- (Generality)
- `PREEMPT_RT` at heart of Kernel
- May not roadblock Kernel development

</v-clicks>

<footer class="text-[10px] mt-auto font-thin">

\[0] https://www.linux.com/news/in-the-trenches-with-thomas-gleixner-real-time-linux-kernel-patch-set \
\[2] Federico Reghenzani, Giuseppe Massari, and William Fornaciari. 2019. The Real-Time Linux Kernel: A Survey on PREEMPT_RT. ACM Comput. Surv. 52, 1, Article 18 (February 2019), 36 pages. https://doi.org/10.1145/3297714
</footer>

---

# Difficulties

<figure class="h-104 w-full flex gap-4 justify-center">
  <img src="https://imgs.xkcd.com/comics/dependency.png" class="h-full ml-40"/>
  <figcaption class="mt-auto text-xs"> <a href="https://xkcd.com/2347"> https://xkcd.com/2347 </a> </figcaption>
</figure>

---

# Difficulties

<v-clicks depth=2>

- Funding
- Supporters
- Long-term commitment
- Validating `PREEMPT_RT`
  - LF ELISA

</v-clicks>

<footer class="text-[10px] mt-auto font-thin">

\[0] https://www.linux.com/news/in-the-trenches-with-thomas-gleixner-real-time-linux-kernel-patch-set \
\[2] Federico Reghenzani, Giuseppe Massari, and William Fornaciari. 2019. The Real-Time Linux Kernel: A Survey on PREEMPT_RT. ACM Comput. Surv. 52, 1, Article 18 (February 2019), 36 pages. https://doi.org/10.1145/3297714
</footer>

---

# Advantages

<v-clicks>

- FOSS
- Versatile
- Scalable
- More complex applications
- Quality of Life features
- Accessibility
- It's Linux!


</v-clicks>

<footer class="text-[10px] mt-auto font-thin">

\[0] https://www.linux.com/news/in-the-trenches-with-thomas-gleixner-real-time-linux-kernel-patch-set \
\[2] Federico Reghenzani, Giuseppe Massari, and William Fornaciari. 2019. The Real-Time Linux Kernel: A Survey on PREEMPT_RT. ACM Comput. Surv. 52, 1, Article 18 (February 2019), 36 pages. https://doi.org/10.1145/3297714
</footer>

---

# References

- \[0] https://www.linux.com/news/in-the-trenches-with-thomas-gleixner-real-time-linux-kernel-patch-set
- \[1] A. Casimiro, P. Martins and P. Verissimo, "How to build a timely computing base using real-time Linux," 2000 IEEE International Workshop on Factory Communication Systems. Proceedings (Cat. No.00TH8531), Porto, Portugal, 2000, pp. 127-134, doi: 10.1109/WFCS.2000.882542.
- \[2] Federico Reghenzani, Giuseppe Massari, and William Fornaciari. 2019. The Real-Time Linux Kernel: A Survey on PREEMPT_RT. ACM Comput. Surv. 52, 1, Article 18 (February 2019), 36 pages. https://doi.org/10.1145/3297714
- \[3] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/sched_policy_prio/sched_deadline
- \[4] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/sched_rt_throttling
- \[5] https://wiki.linuxfoundation.org/realtime/documentation/technical_details/start
- \[6] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/preemption_models
- \[7] https://lwn.net/Articles/146861/
- \[8] https://lwn.net/ml/linux-kernel/CAHk-%3Dwie%2BVC-R5%3DHm%3DVrg5PLrJxb1XiV67Efx-9Cr1fBKCWHTQ%40mail.gmail.com
- \[9] https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=c903327d3295b135eb8c81ebe0b68c1837718eb8

---
layout: 
---

<figure class="flex flex-col items-center h-full justify-center">
  <img src="/qr-code.svg" class="w-60"/>
  <figcaption><a href="https://t1p.de/rt-linux">https://t1p.de/rt-linux</a></figcaption>
</figure>