<template>
  <div v-if="favorites.length" class="mt-6">
    <h2 class="font-semibold mb-2">⭐ 我的收藏站點：</h2>
    <ul class="space-y-1">
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
defineProps({
favorites: Array
})
</script>
  