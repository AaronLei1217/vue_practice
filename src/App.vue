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

<script>
import cityData from './city.json'

export default {
  name: 'CityDistrictSelector',
  data() {
    return {
      selectedCity: '',
      cities: cityData,
    }
  },
  computed: {
    selectedCityDistricts() {
      const city = this.cities.find(city => city.id === this.selectedCity)
      if (city) {
        return city.districts.sort((a, b) => a.sort - b.sort)
      } else {
        console.error(`City with id "${this.selectedCity}" not found`)
        return []
      }
    },
    selectedCityName() {
      const city = this.cities.find(city => city.id === this.selectedCity)
      return city ? city.name : ''
    }
  }
}
</script>