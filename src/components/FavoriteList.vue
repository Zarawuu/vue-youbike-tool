<template>
  <div v-if="favorites.length" class="mt-6">
    <h2 class="flex items-center font-semibold mb-2">
      <img
        :src="starIcon"
        alt="img"
        class="mr-1"
      />
      我的收藏站點：
    </h2>
    <ul class="ml-1 space-y-1 list-decimal list-inside">
      <li
        v-for="station in favorites"
        :key="station.sno"
        @click="$emit('select', station)"
        class="cursor-pointer hover:text-stone-800 text-stone-500"
      >
        {{ station.sna }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import starIcon from '@/assets/stars.svg';
defineProps({
favorites: Array
})
</script>
  