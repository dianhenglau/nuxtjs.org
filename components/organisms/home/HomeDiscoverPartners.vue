<template>
  <div class="relative pb-48 sm:pb-56 md:pb-64 lg:pb-72 xl:pb-92 2xl:pb-128 bg-sky-surface">
    <section class="relative pt-20">
      <NuxtContainer class="flex flex-col items-center xl:pt-10 text-sky-black">
        <div class="flex flex-col w-full items-center col-span-12">
          <div class="mb-2">
            <span class="text-tertiary font-bold text-lg">{{ category }}</span>
          </div>
          <h2 class="font-normal font-serif text-center text-display-6 md:text-display-5 2xl:text-display-4 mb-2">
            <Markdown use="title" unwrap="p" />
          </h2>
          <p class="font-normal text-center text-body-base md:text-body-lg 2xl:text-body-xl mb-8">
            <Markdown use="description" unwrap="p" />
          </p>
          <PartnersBanner class="pb-0" :partners-logo="partners" />
          <p><Markdown use="bottom" unwrap="p" /></p>
        </div>
      </NuxtContainer>
    </section>
    <img
      loading="lazy"
      :src="`/img/home/discover/partners/partners-illustration.svg`"
      class="absolute left-0 bottom-0 z-10 object-fill w-full -mt-28 sm:-mt-40 lg:-mt-60 xl:-mt-80"
      alt="A landscape image"
    />
  </div>
</template>

<script>
import { defineComponent, ref, useContext, useFetch } from '@nuxtjs/composition-api'

export default defineComponent({
  props: {
    category: {
      type: String,
      default: 'Category'
    }
  },
  setup() {
    const { $docus, i18n } = useContext()
    const partners = ref()

    useFetch(async () => {
      partners.value = await $docus
        .search('/partners', { deep: true })
        .where({ slug: { $ne: '' }, language: i18n.locale })
        .fetch()
    })

    return {
      partners
    }
  }
})
</script>
