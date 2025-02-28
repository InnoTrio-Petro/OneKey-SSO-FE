<script setup lang="ts">
import gsap from "gsap";
import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';
import type { Content } from "@prismicio/client";

// The array passed to `getSliceComponentProps` is purely optional.
// Consider it as a visual hint for you when templating your slice.
defineProps(
  getSliceComponentProps<Content.ShowcaseSlice>([
    "slice",
    "index",
    "slices",
    "context",
  ]),
);

onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce').matches;

  if (prefersReducedMotion) return;

  gsap.registerPlugin(ScrollTrigger);

  gsap.fromTo(
    '.showcase__heading',
    {
      y: 100
    },
    {
      y: 0,
      ease: 'power2.inOut',
      duration: 1,
      scrollTrigger: {
        trigger: '.showcase__heading',
        start: 'top bottom-=40%',
        toggleActions: 'play pause resume reverse'
      }
    }
  );

  gsap.fromTo(
    '.showcase__glow',
    {
      scale: 0.7,
      opacity: 0.1
    },
    {
      scale: 1,
      opacity: 1,
      ease: 'power2.inOut',
      duration: 1,
      scrollTrigger: {
        trigger: '.showcase__heading',
        start: 'top bottom-=40%',
        toggleActions: 'play pause resume reverse'
      }
    }
  );
});
</script>

<template>
  <Bounded data-slice-type="slice.slice_type" data-slice-variation="slice.variation" class="relative">
    <div
      class="showcase__glow absolute -z-10 aspect-video w-full max-w-2xl rounded-full bg-sky-700 mix-blend-screen blur-[120px] filter"
    />
    <PrismicRichText
      :field="slice.primary.heading"
      class="showcase__heading text-balance text-center text-5xl font-medium md:text-7xl"
      wrapper="header"
    />
    <div
      class="relative mt-16 grid items-center gap-8 rounded-xl border border-sky-50/20 bg-gradient-to-b from-gray-50/15 to-gray-50/5 px-8 py-8 backdrop-blur-sm lg:grid-cols-3 lg:gap-0 lg:py-12"
    >
      <div class="grid-background" />
      <div>
        <figure class="w-fit rounded-lg bg-sky-900 p-4 text-3xl">
          <Icon :name="`ph:${slice.primary.icon}`" class="block" />
        </figure>
        <PrismicText
          :field="slice.primary.subheading"
          class="mt-6 text-2xl font-normal"
          wrapper="h3"
        />
        <PrismicRichText
          :field="slice.primary.body"
          class="prose prose-invert mt-4 max-w-xl"
          wrapper="div"
        />
        <PrismicLink :field="slice.primary.cta" class="buttonLink mt-6" />
      </div>
      <PrismicImage
        :field="slice.primary.image"
        class="opacity-90 shadow-2xl lg:col-span-2 lg:pt-0"
        :class="slice.variation === 'reversed' ? 'lg:order-1 lg:translate-x-[15%]' : 'lg:-order-1 lg:translate-x-[-15%]'"
      />
    </div>
  </Bounded>
</template>

<style scoped>
.grid-background {
  background-image: url('/assets/grid-pattern.png');
  position: absolute;
  inset: 0;
  background-repeat: repeat;
  z-index: -1;
  background-position: center;
  opacity: 0.15;
  mask-image: radial-gradient(circle at 60% 50%, black 10%, transparent 40%);
}
</style>