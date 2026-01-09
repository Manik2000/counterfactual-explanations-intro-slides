<script setup lang="ts">
import { computed } from 'vue'
import { useNav, useSlideContext } from '@slidev/client'

const nav = useNav()
const context = useSlideContext()

const figureNumber = computed(() => {
  const currentSlide = nav.currentSlideNo.value

  // Count all <FigureNumber/> components in slides before the current one
  let count = 0

  // Get all slides
  const slides = nav.slides.value

  // Count figures in all previous slides
  for (let i = 0; i < currentSlide - 1; i++) {
    const slide = slides[i]
    if (slide?.raw) {
      // Count occurrences of <FigureNumber in the slide's raw content
      const matches = slide.raw.match(/<FigureNumber/g)
      if (matches) {
        count += matches.length
      }
    }
  }

  // Increment for current figure
  count++

  return count
})
</script>

<template>
  <span>Figure {{ figureNumber }}. </span>
</template>
