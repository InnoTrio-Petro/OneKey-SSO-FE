<script setup lang="ts">
import gsap from "gsap";
import type { Content } from "@prismicio/client";

const prismic = usePrismic()
// The array passed to `getSliceComponentProps` is purely optional.
// Consider it as a visual hint for you when templating your slice.
const props = defineProps(
  getSliceComponentProps<Content.IntegrationsSlice>([
    "slice",
    "index",
    "slices",
    "context",
  ]),
);

const integrations = computed(() => {
  return props.slice.primary.integrations
    .map((integration) => integration.icon_id)
    .filter((icon) => prismic.isFilled.keyText(icon))
});

onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce').matches;

  if (prefersReducedMotion) return;

  const tl = gsap.timeline({
    repeat: -1,
    defaults: { ease: 'power2.inOut' }
  });

  tl.to('.pulsing-logo', {
    keyframes: [
      {
        filter: 'brightness(2)',
        opacity: 1,
        duration: 0.4,
        ease: 'power2.in'
      },
      { filter: 'brightness(1)', opacity: 0.7, duration: 0.9 }
    ]
  });

  tl.to(
    '.signal-line',
    {
      keyframes: [
        { backgroundPosition: '0% 0%' },
        {
          backgroundPosition: '100% 100%',
          stagger: { from: 'center', each: 0.3 },
          duration: 1
        }
      ]
    },
    '-=1.4'
  );

  tl.to(
    '.pulsing-icon',
    {
      keyframes: [
        {
          opacity: 1,
          duration: 1,
          stagger: {
            from: 'center',
            each: 0.3
          }
        },
        {
          opacity: 0.4,
          duration: 1,
          stagger: {
            from: 'center',
            each: 0.3
          }
        }
      ]
    },
    '-=2'
  );
});
</script>

<template>
  <Bounded
    :data-slice-type="slice.slice_type"
    :data-slice-variation="slice.variation"
    class="relative overflow-hidden"
  >
    <img src="/assets/gradient.png" alt="" class="absolute inset-0 h-full w-full object-cover">
    <GlideLogoFrame class="absolute left-1/2 top-1/2 -translate-x-[50%] -translate-y-1/2 scale-150 opacity-60" />
    <GlideLogoFrame class="absolute left-1/2 top-1/2 -translate-x-[120%] -translate-y-1/3 scale-150 opacity-60" />
    <GlideLogoFrame class="absolute left-1/2 top-1/2 translate-x-[20%] -translate-y-2/3 scale-150 opacity-60" />

    <div class="relative">
      <PrismicText
        :field="slice.primary.heading"
        wrapper="h2"
        class="mx-auto max-w-2xl text-balance bg-gradient-to-b from-sky-50 to-sky-300 bg-clip-text py-2 text-center text-5xl font-medium text-transparent md:text-7xl"
      />
      <PrismicRichText
        :field="slice.primary.body"
        class="mx-auto mt-6 max-w-md text-balance text-center text-gray-300"
        wrapper="div"
      />

      <div class="mt-20 flex flex-col items-center md:flex-row">
        <template v-for="(integration, index) in integrations" :key="integration">
          <template v-if="index === Math.floor(integrations.length / 2)">
            <GlideLogoStylized class="pulsing-logo shrink-0 opacity-70 brightness-100 -mt-[42px] -mb-[11px] md:mb-4 md:mt-0 md:-mr-10 md:-ml-7" />
            <div class="signal-line rotate-180" />
          </template>
          <div
            class="pulsing-icon flex aspect-square shrink-0 items-center justify-center rounded-full border border-sky-50/30 bg-sky-50/25 p-4 text-3xl text-sky-100 opacity-40 md:text-3xl lg:text-5xl"
          >
            <Icon :name="integration" />
          </div>
          <div
            v-if="index !== integrations.length - 1"
            class="signal-line"
            :class="index >= Math.floor(integrations.length / 2) ? 'rotate-180' : 'rotate-0'"
          />
        </template>
      </div>
    </div>
  </Bounded>
</template>

<style scoped>
.signal-line {
  --rotation: 90deg;
  @apply h-[20px] w-[1.5px] bg-gradient-to-t md:h-[1.5px] md:w-[30px] lg:w-[40px] xl:w-[50px];
  background-color: hsla(0, 0%, 100%, 0.1);
  background-image: linear-gradient(
    var(--rotation),
    rgba(255, 255, 255, 0) 50%,
    #0ea5e9 50%,
    rgba(255, 255, 255, 0) 70%
  );
  background-size: 500% 500%;
}

@media (max-width: 767px) {
  .signal-line {
    --rotation: 0deg;
  }
}
</style>