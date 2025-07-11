<template>
  <div class="p-6 max-w-3xl mx-auto">
    <h1 class="text-2xl font-bold mb-4">🚲 YouBike 即時查詢</h1>
    <SearchBar 
      @search="handleSearch" 
      @cancel="handleCancelSearch"  
    />
    <StationList
      :stations="filteredStations"
      :favorites="favorites"
      @select="selectStation"
      @toggle-favorite="toggleFavorite"
    />
    <FavoriteList :favorites="favorites" @select="selectStation" />
    <StationDetail v-if="selectedStation" :station="selectedStation" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import SearchBar from './components/SearchBar.vue'
import StationList from './components/StationList.vue'
import StationDetail from './components/StationDetail.vue'
import FavoriteList from './components/FavoriteList.vue'

const stations = ref([])
const filteredStations = ref([])
const selectedStation = ref(null)
const favorites = ref([])

const fetchYouBikeData = async () => {
  const res = await axios.get('https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.json')
  stations.value = Object.values(res.data.retVal)
}

const handleSearch = (keyword) => {
  const k = keyword.trim().toLowerCase()
  filteredStations.value = stations.value.filter(
    s => s.sna.toLowerCase().includes(k) || s.ar.toLowerCase().includes(k)
  )
  selectedStation.value = null
}

const handleCancelSearch = () => {
  filteredStations.value = []
  selectedStation.value = null
}

const selectStation = (station) => {
  selectedStation.value = station
}

const loadFavorites = () => {
  const saved = localStorage.getItem('favorites')
  favorites.value = saved ? JSON.parse(saved) : []
}

const saveFavorites = () => {
  localStorage.setItem('favorites', JSON.stringify(favorites.value))
}

const toggleFavorite = (station) => {
  const index = favorites.value.findIndex(s => s.sno === station.sno)
  if (index > -1) {
    favorites.value.splice(index, 1)
  } else {
    favorites.value.push(station)
  }
  saveFavorites()
}

onMounted(() => {
  fetchYouBikeData()
  loadFavorites()
})
</script>