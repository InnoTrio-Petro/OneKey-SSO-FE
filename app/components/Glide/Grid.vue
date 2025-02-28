<script lang="ts" setup>
import gsap from "gsap"

const grid = [14, 30];

onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce').matches;

  if (prefersReducedMotion) {
    gsap.set('#glide-grid', { opacity: 1 });
    gsap.set('.glide-grid-item', {
      opacity: 0.2,
      scale: 1
    });
    return;
  }

  gsap.set('.glide-grid-item', {
    opacity: 0,
    transformOrigin: 'center',
    color: '#fff'
  });

  gsap.set('#glide-grid', { opacity: 1 });

  const tl = gsap.timeline();

  // Entrance Animation
  tl.to('.glide-grid-item', {
    keyframes: [
      {
        opacity: 0,
        duration: 0
      },
      {
        opacity: 0.4,
        rotate: '+=180',
        color: '#0284c7',
        scale: 3,
        duration: 0.6,
        stagger: {
          amount: 2,
          grid: grid,
          from: 'center'
        }
      },
      {
        opacity: 0.2,
        rotate: '+=180',
        color: '#fff',
        scale: 1,
        delay: -2,
        duration: 0.6,
        stagger: {
          amount: 3,
          grid: grid,
          from: 'center'
        }
      }
    ]
  });

  // Loop Animation
  tl.to('.glide-grid-item', {
    delay: 12,
    repeat: -1,
    repeatDelay: 12,
    keyframes: [
      {
        opacity: 0.4,
        rotate: '+=180',
        color: '#0284c7',
        scale: 3,
        duration: 0.6,
        stagger: {
          amount: 2,
          grid: grid,
          from: 'center'
        }
      },
      {
        opacity: 0.2,
        rotate: '+=180',
        color: '#fff',
        scale: 1,
        delay: -2,
        duration: 0.6,
        stagger: {
          amount: 3,
          grid: grid,
          from: 'center'
        }
      }
    ]
  });
});
</script>

<template>
  <svg
    id="glide-grid"
    xmlns="http://www.w3.org/2000/svg"
    fill="none"
    viewBox="0 0 935 425"
    class="absolute -left-2 -top-14 -z-10"
    opacity="0"
    style="mask-image: linear-gradient(black, transparent);"
  >
    <g class="glide-grid-group">
      <template v-for="i in grid[0]" :key="i">
        <path
          v-for="j in grid[1]"
          :key="j"
          fill="currentColor"
          opacity=".2"
          class="glide-grid-item"
          :d="`M${(j - 1) * 32 + 5},${(i - 1) * 32 + 10}l1.806,-2.951l-5,2.951l3.936,1.049l-0.742,-1.049z`"
        />
      </template>
    </g>
  </svg>
</template>