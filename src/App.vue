<template>
  <div class="p-6 max-w-3xl mx-auto">
    <div class="flex justify-between items-center mb-4">
      <h1 class="flex items-center text-2xl font-bold">
        <img
         :src="youbikeIcon"
         alt="img"
         class="w-20 h-20 mr-1"
        />
        即時查詢
      </h1>
      <div 
        class="tooltip tooltip-info tooltip-left" 
        data-tip="按下愛心可以收藏或取消收藏站點"
      >
        <button class="btn btn-sm btn-circle btn-info">?</button>
      </div>
    </div>
    <SearchBar 
      @search="handleSearch" 
      @cancel="handleCancelSearch"  
    />
    <StationList
      :stations="filteredStations"
      :favorites="favorites"
      :expandedSno="expandedSno"
      @select="selectStation"
      @toggle-favorite="toggleFavorite"
      @toggle-expand="toggleExpand"
    />
    <FavoriteList :favorites="favorites" @select="selectStation" />
    <Transition name="slide-left-fade" mode="out-in">
      <StationDetail 
        v-if="selectedStation" 
        :station="selectedStation" 
        :key="selectedStation.sno"  
      />
    </Transition>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import youbikeIcon from '@/assets/youbike.png'
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
}

const handleCancelSearch = () => {
  filteredStations.value = []
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
    selectedStation.value = null
  } else {
    favorites.value.push(station)
  }
  saveFavorites()
}

const expandedSno = ref(null)

const toggleExpand = (sno) => {
  expandedSno.value = expandedSno.value === sno ? null : sno
}

onMounted(() => {
  fetchYouBikeData()
  loadFavorites()
})
</script>

<style scoped>
.slide-left-fade-enter-active,
.slide-left-fade-leave-active {
  transition: all 0.4s ease;
}

.slide-left-fade-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

.slide-left-fade-enter-to {
  opacity: 1;
  transform: translateX(0);
}

.slide-left-fade-leave-from {
  opacity: 1;
  transform: translateX(0);
}

.slide-left-fade-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}
</style>