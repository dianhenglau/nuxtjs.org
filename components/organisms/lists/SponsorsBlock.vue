<template>
  <div class="flex flex-col">
    <p class="text-base font-semibold text-gray-900 dark:text-gray-100">{{ $t('sidebar.partners.title') }}</p>
    <ul class="py-2 flex flex-col space-y-2 ml-4 mb-2">
      <li v-for="partner in partners" :key="partner.title" class="">
        <a
          :href="partner.url"
          class="group flex font-medium items-center hover:opacity-100 d-secondary-text hover:d-secondary-text-hover"
          rel="noopener sponsored"
          target="_blank"
        >
          {{ partner.title }}
          <IconExternalLink class="w-4 h-4 ml-1.5 opacity-0 group-hover:opacity-100" />
        </a>
      </li>
    </ul>

    <div class="pt-2">
      <NuxtButton
        type="button"
        :to="localePath('/sponsors')"
        class="
          rounded-md
          bg-primary
          text-gray-800
          hover:bg-primary-300
          focus:bg-primary-300
          inline-flex
          items-center
          px-3
          py-1.5
          text-xs
          2xl:text-sm
          text-center
        "
      >
        {{ $t('sidebar.partners.button') }}
      </NuxtButton>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, useContext, useFetch } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const { $docus } = useContext()
    const partners = ref()

    useFetch(async () => {
      partners.value = await $docus
        .search('/sponsors', { deep: true })
        .where({ tier: { $in: ['MVP Partners', 'Partners'] } })
        .fetch()
    })

    return {
      partners
    }
  }
})
</script>
