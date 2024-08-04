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
import {ref, computed, onMounted} from 'vue'
import axios from 'axios';

const cities = ref([])
const selectedCity = ref('')

const fetchCityData = async () => {
  try {
    const response = await axios.get('https://raw.githubusercontent.com/kurotanshi/mask-map/master/raw/area-location.json')

    // 給定預設值是台北 如果再宣告selectedCity就設定為台北 會抓不到 因為還沒有獲取 api 的資料
    selectedCity.value = response.data[0].id
    cities.value = response.data
  } catch (err) {
    console.error('Error fetching data:', err)
  }
}
// async await 寫法 與一般promise 寫法 
// axios 這個 library  回傳的是 promise 物件

// const fetchCityData = () => {
//   axios.get('https://raw.githubusercontent.com/kurotanshi/mask-map/master/raw/area-location.json')
//     .then(response => {
//       cities.value = response.data
//     })
//     .catch(err => {
//       console.error('Error fetching data:', err)
//     })
// }

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

// 通常會把 hook 放在最後面
onMounted(fetchCityData)

</script>