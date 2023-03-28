<template>
  <main class="flex flex-col justify-center h-full mx-auto max-w-600px">
    <section class="flex flex-col items-center leading-loose text-center">
      <div class="text-3xl">
        <span class="i-twemoji-christmas-tree"></span>
        {{ t('happyHolidays') }}
        <span class="i-twemoji-world-map"></span>
      </div>
      <!-- Dates - Check out locales/en.json for the key -->
      <i18n-t keypath="christmasIsComing" tag="div">
        <template #date>
          <span>{{ d(christmasDate, 'long') }}</span>
        </template>
        <template #time>
          <span class="text-green-400">{{
            t('day', {
              count: daysUntilChristmas,
            })
          }}</span>
        </template>
      </i18n-t>
      <!-- Controls - I give you an .icon-button class if you want to use it -->
      <div class="flex justify-between w-200px">
        <button class="icon-button" @click="switchLocale">
          <span class="i-carbon-language" />
        </button>
        <!-- Flags - the current locale -->
        <div>
          <span :class="flags[locale]"></span>
          {{ t('language') }}
        </div>
      </div>
    </section>
  </main>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useI18n } from 'vue-i18n'

const { t, d, locale, availableLocales } = useI18n()

const currentLocale = ref(availableLocales[0])

const switchLocale = () => {
  const currentIndex = availableLocales.findIndex(
    locale => locale === currentLocale.value
  )

  if (currentIndex === availableLocales.length - 1) {
    currentLocale.value = availableLocales[0]
  } else {
    currentLocale.value = availableLocales[currentIndex + 1]
  }
  locale.value = currentLocale.value
}

const flags = {
  en: 'i-twemoji-flag-united-states',
  de: 'i-twemoji-flag-germany',
  'ja-JP': 'i-twemoji-flag-japan',
}

// See the README about tricky timezone issues!
// I figured since this is i18n-friendly, we'd wanna
// make sure the timezones were right :-)
const christmasDate = new Date('2023/12/25')

const daysUntilChristmas = Math.ceil(
  (christmasDate.getTime() - new Date().getTime()) / (1000 * 3600 * 24)
)
</script>

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
