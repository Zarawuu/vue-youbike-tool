<template>
  <div class="flex gap-2 mb-4">
    <input
      v-model="keyword"
      @keyup.enter="emitSearch"
      type="text"
      placeholder="輸入站名或地址"
      class="input grow"
    />
    <button @click="emitSearch" class="btn btn-success">搜尋</button>
    <button @click="cancelSearch" class="btn btn-error">取消搜尋</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const keyword = ref('')
const emit = defineEmits(['search', 'cancel'])

const emitSearch = () => {
  emit('search', keyword.value)
}

const cancelSearch = () => {
  emit('cancel')
}
</script>
  