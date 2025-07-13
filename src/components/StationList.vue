<template>
  <div v-if="stations.length">
    <h2 class="font-semibold mb-2">查詢結果：</h2>
    <ul class="space-y-2">
      <li
        v-for="station in stations"
        :key="station.sno"
        class="border border-gray-300 p-3 rounded"
      >
        <div class="flex justify-between items-center">
          <div class="cursor-pointer" @click="$emit('toggle-expand', station.sno)">
            <p class="font-medium">{{ station.sna }}</p>
            <p class="text-sm text-gray-600">{{ station.ar }}</p>
          </div>
          <button class="btn btn-square cursor-pointer" @click.stop="$emit('toggle-favorite', station)">
            <img
              :src="isFavorited(station.sno) ? heartIcon : heartBrokenIcon"
              alt="img"
              class="w-6 h-6"
            />
          </button>
        </div>
        <Transition name="fade-slide">
          <div v-if="station.sno === expandedSno" class="mt-3 border-t pt-3 text-sm text-gray-300">
            <h3 class="font-semibold text-base mb-2 text-gray-800">📊 即時狀態</h3>
            <p class="flex items-center text-gray-700">
              可借車輛：{{ station.sbi }}
            </p>
            <p class="flex items-center text-gray-700">
              可停空位：{{ station.bemp }}
            </p>
            <p class="flex items-center text-gray-700">
              總停車格：{{ station.tot }}
            </p>
          </div>
        </Transition>
      </li>
    </ul>
  </div>
</template>

<script setup>
import heartIcon from '@/assets/heart.svg'
import heartBrokenIcon from '@/assets/heart-break.svg'

const props = defineProps({
  stations: Array,
  favorites: Array,
  expandedSno: String
})

const isFavorited = (sno) => {
  return props.favorites.some(s => s.sno === sno)
}
</script>

<style scoped>
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all .3s ease;
  overflow: hidden;
}
.fade-slide-enter-from,
.fade-slide-leave-to {
  opacity: 0;
  margin-top: 0;
  padding-top: 0;
  max-height: 0px;
}
.fade-slide-enter-to,
.fade-slide-leave-from {
  opacity: 1;
  margin-top: 0.75rem;
  padding-top: 0.75rem;
  max-height: 200px;
}
</style>