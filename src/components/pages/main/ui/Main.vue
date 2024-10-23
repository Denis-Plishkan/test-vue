<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import axios from 'axios'
import { UICard, Widget } from '@/components'
import BaseInput from '@/components/Base/input/BaseInput.vue'

const countries = ref([])
const filteredCountries = ref([])
const error = ref('')
const currentPage = ref<number>(1)
const itemsPerPage = ref<number>(6)
const totalItems = ref<number>(0)
const paginatedCountries = ref([])
const search = ref<string>('')

const API_URL = import.meta.env.VITE_API_URL

const fetchAvailableCountries = async () => {
  try {
    const response = await axios.get(`${API_URL}/AvailableCountries`)
    countries.value = response.data
    filteredCountries.value = countries.value
    totalItems.value = filteredCountries.value.length
    paginateCountries()
  } catch (err) {
    error.value = 'Failed to fetch countries: ' + err
  }
}

const paginateCountries = () => {
  const startIndex = (currentPage.value - 1) * itemsPerPage.value
  const endIndex = startIndex + itemsPerPage.value
  paginatedCountries.value = filteredCountries.value.slice(startIndex, endIndex)
}

watch(search, () => {
  filterCountries()
})

const filterCountries = () => {
  if (search.value) {
    filteredCountries.value = countries.value.filter(
      (country: { name: string }) =>
        country.name.toLowerCase().includes(search.value.toLowerCase()),
    )
  } else {
    filteredCountries.value = countries.value
  }
  totalItems.value = filteredCountries.value.length
  currentPage.value = 1
  paginateCountries()
}

watch(currentPage, () => {
  paginateCountries()
})

onMounted(() => {
  fetchAvailableCountries()
})
</script>

<template>
  <section class="main">
    <div class="main__container">
      <h1>Main Page</h1>
      <div v-if="error">{{ error }}</div>

      <div class="main__input">
        <BaseInput v-model="search" placeholder="Search countries..." />
      </div>

      <div class="main__card-wrapper">
        <div class="main__card" v-if="paginatedCountries.length > 0">
          <UICard
            v-for="country in paginatedCountries"
            :key="country.countryCode"
            :name="country.name"
            :countryCode="country.countryCode"
          />
        </div>
        <div class="main__card" v-else>
          <h2 class="main__card-title">The list is empty !</h2>
        </div>

        <Widget />
      </div>

      <div class="main__pagination">
        <v-pagination
          v-model="currentPage"
          :length="Math.ceil(totalItems / itemsPerPage)"
          @input="paginateCountries"
          :total-visible="5"
        />
      </div>
    </div>
  </section>
</template>

<style lang="scss">
@use 'Main';
</style>
