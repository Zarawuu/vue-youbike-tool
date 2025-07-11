<template>
<div class="card bg-base-300 w-96 shadow-sm mt-6">
  <div class="card-body">
    <h2 class="card-title">📍 {{ station.sna }}</h2>
    <p class="flex items-center">
      <img
        :src="locationIcon"
        alt="img"
        class="w-4 h-4 mr-1"
      />
      {{ station.ar }}
    </p>
    <p class="flex items-center">
      <img
        :src="bikeIcon"
        alt="img"
        class="w-4 h-4 mr-1"
      />
      {{ station.sbi }}
    </p>
    <p class="flex items-center">
      <img
        :src="bikeDockIcon"
        alt="img"
        class="w-4 h-4 mr-1"
      />
      {{ station.bemp }}
    </p>
    <p class="flex items-center">
      <img
        :src="parkingIcon"
        alt="img"
        class="w-4 h-4 mr-1"
      />
      {{ station.tot }}
    </p>
  </div>
</div>
</template>

<script setup>
import locationIcon from '@/assets/location.svg'
import bikeDockIcon from '@/assets/bike-dock.svg'
import bikeIcon from '@/assets/bike.svg'
import parkingIcon from '@/assets/parking.svg'

defineProps({
  station: Object
})
</script>
  