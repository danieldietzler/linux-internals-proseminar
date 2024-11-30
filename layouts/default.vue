<template>
  <div class="slidev-layout layout flex flex-col">
    <slot />
  </div>
  <footer
    v-if="$slidev.nav.currentLayout !== 'cover'"
    class="absolute flex bottom-0 w-full m-4 justify-between pr-10 font-light text-sm"
  >
    <div>Daniel Dietzler</div>
    <div class="font-normal">{{ $slidev.nav.currentPage }}/{{ $slidev.nav.total }}</div>
  </footer>
</template>