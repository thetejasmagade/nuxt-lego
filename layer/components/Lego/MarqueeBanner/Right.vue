<script setup lang="ts">
import { ref } from 'vue'
import { rootKey } from './.keys'
import { useMarqueeBanner } from './.composables'

const rootInject = inject(rootKey, {
  speed: 1,
  hoveredSpeed: 0.5,
})

const el = ref<HTMLElement[]>()
const offset = ref(0)
const intervalId = ref()

const { componentsWidth, baseEl, shadowEl, isHovering, onMouseEnter, onMouseLeave } = useMarqueeBanner(el)

onMounted(() => {
  intervalId.value = setInterval(() => {
    if (Math.abs(offset.value) >= (componentsWidth.value)) {
      offset.value = 0
    }

    else {
      if (isHovering.value)
        offset.value += rootInject.hoveredSpeed

      else
        offset.value += rootInject.speed
    }
  }, 5)
})

onUnmounted(() => {
  clearInterval(intervalId.value)
})
</script>

<template>
  <div style="display: flex; position: relative;  float: right" :style="{ transform: `translateX(${offset}px)` }" @mouseenter="onMouseEnter" @mouseleave="onMouseLeave">
    <component :is="component" v-for="(component, index) in shadowEl" :key="index" />
    <component :is="component" v-for="(component, index) in baseEl" ref="el" :key="index" />
  </div>
</template>
