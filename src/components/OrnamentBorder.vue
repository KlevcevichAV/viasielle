<template>
  <div
    class="ornament-border"
    :style="{ height: height + 'px', backgroundImage: `url(${ornamentTile})`, backgroundSize: `auto ${height}px` }"
    aria-hidden="true"
  />
</template>

<script setup>
import ornamentTile from '@/assets/decor/ornament-tile.png'

defineProps({
  height: { type: [Number, String], default: 72 },
})
</script>

<style scoped>
.ornament-border {
  position: relative;
  width: 100%;
  z-index: 1;
  background-repeat: repeat-x;
  background-position: center;
}

[data-theme='dark'] .ornament-border {
  filter: brightness(1.2);
}
</style>
