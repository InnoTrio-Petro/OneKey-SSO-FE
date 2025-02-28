<script setup lang="ts">
import type { Content } from "@prismicio/client";

// The array passed to `getSliceComponentProps` is purely optional.
// Consider it as a visual hint for you when templating your slice.
defineProps(
  getSliceComponentProps<Content.CallToActionSlice>([
    "slice",
    "index",
    "slices",
    "context",
  ]),
);
</script>

<template>
  <Bounded
    :data-slice-type="slice.slice_type"
    :data-slice-variation="slice.variation"
    class="relative py-32 text-center font-medium md:py-40"
  >
    <div class="glow absolute -z-10 aspect-square w-full max-w-sm rounded-full bg-sky-500/50 blur-[160px] filter" />
    <div class="glass-container rounded-lg bg-gradient-to-b from-gray-800 to-gray-900 p-4 md:rounded-xl">
      <GlideIcon class="w-24 h-24" />
    </div>
    <PrismicText :field="slice.primary.heading" wrapper="h2" class="mt-8 max-w-xl text-balance text-5xl" />
    <div class="flex gap-8 flex-wrap">
      <PrismicLink v-for="cta in slice.primary.ctas" :key="cta.text" :field="cta" class="buttonLink mt-6" />
    </div>
  </Bounded>
</template>
