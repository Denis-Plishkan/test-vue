<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'
import { UICard } from '@/components'
import { yearData } from '@/data/YearData'

const { years } = yearData()

const route = useRoute()
const countryCode = route.query.code

const holidays = ref([])
const paginatedHolidays = ref([])
const error = ref('')
const currentYear = new Date().getFullYear()
const selectedYear = ref<number>(currentYear)
const currentPage = ref<number>(1)
const itemsPerPage = ref<number>(4)
const totalItems = ref<number>(0)

const API_URL = import.meta.env.VITE_API_URL

const fetchPublicHolidays = async (year: number) => {
  if (countryCode) {
    try {
      const response = await axios.get(
        `${API_URL}/PublicHolidays/${year}/${countryCode}`,
      )
      holidays.value = response.data
      totalItems.value = holidays.value.length
      paginateHolidays()
    } catch (err) {
      error.value = 'Failed to fetch holidays: ' + err
    }
  }
}

const paginateHolidays = () => {
  const startIndex = (currentPage.value - 1) * itemsPerPage.value
  const endIndex = startIndex + itemsPerPage.value
  paginatedHolidays.value = holidays.value.slice(startIndex, endIndex)
}

const selectYear = (year: number) => {
  selectedYear.value = year
  fetchPublicHolidays(year)
}

watch(currentPage, () => {
  paginateHolidays()
})

onMounted(() => {
  fetchPublicHolidays(currentYear)
})

const breadcrumbs = [
  {
    title: 'Main page',
    disabled: false,
    href: '/',
  },
  {
    title: 'Country Page',
    disabled: true,
  },
]
</script>

<template>
  <section class="country">
    <div class="country__container">
      <v-breadcrumbs :items="breadcrumbs"></v-breadcrumbs>

      <h1>Country page</h1>

      <div v-if="error">{{ error }}</div>
      <div class="country__cards" v-if="paginatedHolidays.length > 0">
        <UICard
          v-for="holiday in paginatedHolidays"
          :key="holiday.date"
          :holiday="holiday.name"
          :date="holiday.date"
          :type="holiday.types.join(', ')"
        />

        <div class="country__years-button">
          <v-btn
            v-for="year in years"
            :key="year.year"
            :color="year.year === selectedYear ? 'primary' : 'default'"
            @click="selectYear(year.year)"
          >
            {{ year.year }}
          </v-btn>
        </div>
      </div>
      <div v-else>
        <p>No public holidays found.</p>
      </div>

      <div class="country__pagination" v-if="totalItems > itemsPerPage">
        <v-pagination
          v-model="currentPage"
          :length="Math.ceil(totalItems / itemsPerPage)"
          :total-visible="5"
        />
      </div>
    </div>
  </section>
</template>

<style lang="scss">
@use 'Country';
</style>
