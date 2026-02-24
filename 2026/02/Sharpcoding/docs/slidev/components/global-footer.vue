<template>
  <div class="fixed inset-x-0 bottom-0 flex justify-center pointer-events-none select-none">
    <img :src="footerImage" alt="Global footer" class="w-full max-w-[1100px] h-auto object-contain opacity-95" />
  </div>
</template>

<script setup>
const footerImage = new URL('../images/global-footer.png', import.meta.url).href
</script>

<style>
:root {
  --global-footer-safe-space: 5.5rem;
}

.slidev-layout {
  box-sizing: border-box;
  padding-bottom: var(--global-footer-safe-space);
}
</style>
