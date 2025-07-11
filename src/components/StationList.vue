<template>
  <div v-if="stations.length">
    <h2 class="font-semibold mb-2">查詢結果：</h2>
    <ul class="space-y-2">
      <li
        v-for="station in stations"
        :key="station.sno"
        class="border border-gray-300 p-3 rounded flex justify-between items-center"
      >
        <div>
          <p class="font-medium">{{ station.sna }}</p>
          <p class="text-sm text-gray-600">{{ station.ar }}</p>
        </div>
        <button class="btn btn-square cursor-pointer" @click="$emit('toggle-favorite', station)">
          <img
            :src="isFavorited(station.sno) ? heartIcon : heartBrokenIcon"
            alt="img"
            class="w-6 h-6"
          />
        </button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import heartIcon from '@/assets/heart.svg';
import heartBrokenIcon from '@/assets/heart-break.svg';

const props = defineProps({
  stations: Array,
  favorites: Array
})
const isFavorited = (sno) => {
  return props.favorites.some(s => s.sno === sno)
}
</script>
