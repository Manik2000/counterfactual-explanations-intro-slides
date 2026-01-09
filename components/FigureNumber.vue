<script setup lang="ts">
import { computed } from 'vue'
import { useNav, useSlideContext } from '@slidev/client'

const nav = useNav()
const context = useSlideContext()

const figureNumber = computed(() => {
  const currentSlide = nav.currentSlideNo.value

  let count = 0

  const slides = nav.slides.value

  for (let i = 0; i < currentSlide - 1; i++) {
    const slide = slides[i]
    if (slide?.raw) {
      const matches = slide.raw.match(/<FigureNumber/g)
      if (matches) {
        count += matches.length
      }
    }
  }

  count++
  return count
})
</script>

<template>
  <span>Figure {{ figureNumber }}. </span>
</template>
