<template>
  <div class="slidev-layout layout flex flex-col">
    <slot />
  </div>
  <footer
    v-if="$slidev.nav.currentLayout !== 'cover'"
    class="absolute flex flex-col bottom-0 w-full m-4 justify-between pr-10 font-light text-sm"
  >
    <hr class="w-full"/>
    <div class="flex justify-between">
      <div>Daniel Dietzler</div>
      <div class="font-normal">{{ $slidev.nav.currentPage }}/{{ $slidev.nav.total }}</div>
    </div>
  </footer>
</template>