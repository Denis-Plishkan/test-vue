<script setup lang="ts">
import { useRouter } from 'vue-router'
import { defineProps } from 'vue'

interface Card {
  name?: string
  country?: string
  countryCode?: string
  holiday?: string
  type?: string
  date?: string
}

const { name, country, countryCode, holiday, type, date } = defineProps<Card>()
const router = useRouter()

const goToCountryPage = () => {
  if (countryCode) {
    router.push({ name: 'country', query: { code: countryCode } })
  }
}
</script>

<template>
  <div class="card" @click="goToCountryPage">
    <p v-if="name" class="card__name">
      {{ name }}
    </p>
    <p v-if="country"><span>Country:</span> {{ country }}</p>
    <p v-if="holiday"><span>Holiday:</span> {{ holiday }}</p>
    <p v-if="type"><span>Type:</span> {{ type }}</p>
    <p v-if="date"><span>Date:</span> {{ date }}</p>
  </div>
</template>

<style lang="scss">
@use 'UICard';
</style>
