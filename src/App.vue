<script setup>
import { useI18n } from 'vue-i18n'
import { useCycleList } from '@vueuse/core'
import { watch } from 'vue'
import { computed } from '@vue/reactivity'

const { t, d, locale, availableLocales } = useI18n()
let lang = document.querySelector('html')
const locales = useCycleList(availableLocales)
console.log(locales.state)
watch(locales.state, state => {
  locale.value = state
  lang.setAttribute('lang', locale.value)
})

// make sure the timezones were right :-)
const christmasDate = new Date('2022/12/25')
const daysTillChristmas = computed(() => {
  let daysTo = christmasDate.getTime() - new Date()
  return Math.floor(daysTo / (1000 * 60 * 60 * 24))
})
</script>
<template>
  <main class="flex flex-col justify-center h-full mx-auto max-w-600px">
    <section class="flex flex-col items-center leading-loose text-center">
      <div class="text-3xl">
        <span class="i-twemoji-christmas-tree"></span>
        {{ t('happyHolidays') }}
        <span class="i-twemoji-world-map"></span>
      </div>
      <!-- Dates - Check out locales/en.json for the key -->
      <!-- Had to see how to make this work from the solution. It wasn't intuitive until I tried it! -->
      <i18n-t keypath="christmasIsComing" tag="p" class="date">
        <template #date>
          {{ d(christmasDate, 'short') }}
        </template>
        <template #time>
          {{ t('day', daysTillChristmas) }}
        </template>
      </i18n-t>
      <!-- Controls - I give you an .icon-button class if you want to use it -->
      <button
        class="icon-button"
        @click="locales.next()"
        :title="t('changeLang')"
      >
        <span class="i-carbon-language"></span>
      </button>
      <!-- Flags - the current locale -->
    </section>
  </main>
</template>

<style scoped>
.icon-button {
  @apply text-xl
    w-32px
    h-32px
    rounded-full
    border-1
    border-transparent
    bg-transparent
    cursor-pointer
    duration-300
    hover:ring-2
    hover:border-green-500
    hover:ring-green-500
    hover:ring-opacity-40
    hover:text-green-600;
}
</style>
