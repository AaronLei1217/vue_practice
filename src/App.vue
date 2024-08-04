<template>
  <div>
    <select v-model="selectedCity">
      <option v-for="city in cities" :key="city.id" :value="city.id">
        {{ city.name }}
      </option>
    </select>
    <table>
      <thead>
        <tr>
          <th>縣市</th>
          <th>區域</th>
          <th>郵遞區號</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="district in selectedCityDistricts" :key="district.id">
          <td>{{ selectedCityName }}</td>
          <td>{{ district.name }}</td>
          <td>{{ district.zipCode }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import {ref, computed} from 'vue'
import cityData from './city.json'

const selectedCity = ref('');
const cities = ref(cityData);

const selectedCityDistricts = computed(() => {
  const city = cities.value.find(city => city.id === selectedCity.value)
  if (city) {
    return city.districts.sort((a, b) => a.sort - b.sort)
  } else {
    console.error(`City with id "${selectedCity.value}" not found`)
    return []
  }
})

const selectedCityName = computed(() => {
  const city = cities.value.find(city => city.id === selectedCity.value)
  return city ? city.name : ''
})
</script>