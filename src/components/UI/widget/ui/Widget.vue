<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { UICard } from '@/components'

interface Country {
  countryCode: string
  name: string
}

interface HolidayInfo {
  country: string
  holiday: string
  date: string
}

const countries = ref<Country[]>([])
const selectedCountries = ref<Country[]>([])
const holidaysInfo = ref<HolidayInfo[]>([])
const error = ref<string | null>(null)

const fetchAvailableCountries = async () => {
  try {
    const response = await axios.get(
      `${import.meta.env.VITE_API_URL}/AvailableCountries`,
    )
    countries.value = response.data
    selectRandomCountries()
  } catch (err) {
    error.value = 'Error fetching countries: ' + err
  }
}

const selectRandomCountries = () => {
  const shuffled = countries.value.sort(() => 0.5 - Math.random())
  selectedCountries.value = shuffled.slice(0, 3)
  fetchNextHolidaysForSelected()
}

const fetchNextHolidaysForSelected = async () => {
  try {
    for (const country of selectedCountries.value) {
      const countryCode = country.countryCode
      const countryName = country.name
      const response = await axios.get(
        `${import.meta.env.VITE_API_URL}/NextPublicHolidays/${countryCode}`,
      )

      if (response.data.length > 0) {
        holidaysInfo.value.push({
          country: countryName,
          holiday: response.data[0].localName,
          date: response.data[0].date,
        })
      } else {
        holidaysInfo.value.push({
          country: countryName,
          holiday: 'No upcoming holidays.',
          date: '',
        })
      }
    }
  } catch (err) {
    error.value = 'Error fetching holidays: ' + err
  }
}

onMounted(() => {
  fetchAvailableCountries()
})
</script>

<template>
  <div class="widget__wrapper">
    <div v-if="error" class="error">{{ error }}</div>
    <div class="widget__wrapper-cards" v-if="holidaysInfo.length > 0">
      <UICard
        v-for="(info, index) in holidaysInfo"
        :key="index"
        :country="info.country"
        :holiday="info.holiday"
        :date="info.date"
      />
    </div>
  </div>
</template>

<style lang="scss">
@use 'Widget';
</style>
