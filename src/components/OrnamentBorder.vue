<template>
  <div class="ornament-border" :style="{ height: height + 'px' }" aria-hidden="true">
    <svg width="100%" :height="height" preserveAspectRatio="none">
      <defs>
        <pattern :id="patternId" x="0" y="0" :width="tile" :height="height" patternUnits="userSpaceOnUse">
          <line x1="0" :y1="edge" :x2="tile" :y2="edge" :stroke="accent" stroke-width="1.5" />
          <line x1="0" :y1="height - edge" :x2="tile" :y2="height - edge" :stroke="accent" stroke-width="1.5" />
          <polygon :points="diamond" fill="none" :stroke="color" stroke-width="2.5" />
          <polygon :points="innerDiamond" :fill="accent" />
        </pattern>
      </defs>
      <rect width="100%" :height="height" :fill="`url(#${patternId})`" />
    </svg>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  color: { type: String, default: 'var(--color-primary)' },
  accent: { type: String, default: 'var(--color-secondary)' },
  height: { type: [Number, String], default: 32 },
})

const patternId = `ornament-${Math.random().toString(36).slice(2)}`

const h = computed(() => Number(props.height))
const tile = computed(() => h.value)
const edge = computed(() => Math.max(2, h.value * 0.08))
const mid = computed(() => h.value / 2)
const half = computed(() => h.value * 0.42)
const innerHalf = computed(() => h.value * 0.16)

const diamond = computed(() => {
  const c = mid.value
  const r = half.value
  return `${c},${c - r} ${c + r},${c} ${c},${c + r} ${c - r},${c}`
})

const innerDiamond = computed(() => {
  const c = mid.value
  const r = innerHalf.value
  return `${c},${c - r} ${c + r},${c} ${c},${c + r} ${c - r},${c}`
})
</script>

<style scoped>
.ornament-border {
  position: relative;
  width: 100%;
  z-index: 1;
}

.ornament-border svg {
  display: block;
}
</style>
