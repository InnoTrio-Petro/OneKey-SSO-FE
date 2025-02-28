<script setup lang="ts">
import gsap from 'gsap';
import { components } from '~/slices'

const prismic = usePrismic()
const route = useRoute()
const { data: page } = await useAsyncData(`case-study:${route.params.uid}` as string, () =>
  prismic.client.getByUID('case_study', route.params.uid as string)
)

useSeoMeta({
  title: page.value?.data.meta_title ?? undefined,
  ogTitle: page.value?.data.meta_title ?? undefined,
  description: page.value?.data.meta_description ?? undefined,
  ogDescription: page.value?.data.meta_description ?? undefined,
  ogImage: computed(() => prismic.asImageSrc(page.value?.data.meta_image)),
});

onMounted(() => {
  const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce').matches;

  if (prefersReducedMotion) {
    gsap.set('.case-study__image', { opacity: 1 });
    return;
  }

  gsap.fromTo(
    '.case-study__image',
    {
      opacity: 0,
      y: 100
    },
    {
      opacity: 1,
      y: 0,
      duration: 1,
      delay: 0.5,
      ease: 'power2.inOut'
    }
  );
})
</script>


<template>
  <main>
    <Bounded>
      <header class="relative grid w-full place-items-center text-center">
        <GlideGrid />
        <h1 class="text-5xl font-medium md:text-7xl">
          <PrismicText :field="page?.data.company" />
          <RichTextGlideText class="block text-lg">Case Study</RichTextGlideText>
        </h1>
        <p class="mb-4 mt-8 max-w-xl text-lg text-slate-300">
          <PrismicText :field="page?.data.description" />
        </p>
        <PrismicImage
          v-if="$prismic.isFilled.image(page?.data.cover)"
          :field="page?.data.cover"
          class="case-study__image rounded-lg opacity-0 max-w-4xl w-full"
        />
      </header>
      <SliceZone
        wrapper="div"
        class="mx-auto mt-12 md:mt-16"
        :slices="page?.data.slices ?? []"
        :components="components"
      />
    </Bounded>
  </main>
</template>
