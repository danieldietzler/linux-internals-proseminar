---
# You can also start simply with 'default'
theme: seriph
title: Convex Geometry — Separation Theorems 
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
- Preemptive
- Priority-based process switching
- Verifiable/Provable

</v-clicks>

---

# Requirements of RT Linux

<v-clicks>

- Isolated (best effort)
- "Simple"
- Non-blocking
- Easy to use

</v-clicks>

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

#  RT Linux History: Chaos \[0]

<v-clicks depth=2>

- <2000: early research  \[1]
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

#  RT Linux History: `PREEMPT_RT` \[0]

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

# Current State of RT Linux

<v-clicks>

- `PREEMPT_RT` in Kernel 6.12; November 18th, 2024
- Linux Foundation (LF) ELISA
  - ARM, BMW, RedHat, Intel, Texas Instruments, ...
- `SCHED_DEADLINE` \[3]
- `RT_THROTTLING` \[4]
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

# Difficulties/Complexities

<v-clicks>

</v-clicks>

---

# Advantages

<v-clicks>

</v-clicks>

---

# References

- \[0] https://www.linux.com/news/in-the-trenches-with-thomas-gleixner-real-time-linux-kernel-patch-set
- \[1] A. Casimiro, P. Martins and P. Verissimo, "How to build a timely computing base using real-time Linux," 2000 IEEE International Workshop on Factory Communication Systems. Proceedings (Cat. No.00TH8531), Porto, Portugal, 2000, pp. 127-134, doi: 10.1109/WFCS.2000.882542.
- \[2] Federico Reghenzani, Giuseppe Massari, and William Fornaciari. 2019. The Real-Time Linux Kernel: A Survey on PREEMPT_RT. ACM Comput. Surv. 52, 1, Article 18 (February 2019), 36 pages. https://doi.org/10.1145/3297714
- \[3] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/sched_policy_prio/sched_deadline
- \[4] https://wiki.linuxfoundation.org/realtime/documentation/technical_basics/sched_rt_throttling

---
layout: 
---

<figure class="flex flex-col items-center h-full justify-center">
  <img src="/qr-code.svg" class="w-60"/>
  <figcaption><a href="https://t1p.de/rt-linux">https://t1p.de/rt-linux</a></figcaption>
</figure>