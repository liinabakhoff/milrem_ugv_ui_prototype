<template>
  <div class="popup">
    <button @click="emit('drive')">Drive</button>
    <button @click="emit('save')">Save</button>
    <button @click="emit('discard')">Discard</button>
  </div>
</template>

<script setup lang="ts">
const emit = defineEmits<{
  (e: 'drive'): void
  (e: 'save'): void
  (e: 'discard'): void
}>()
</script>

<style scoped>
.popup {
  position: absolute;
  left: 20px;
  background: white;
  border: 1px solid black;
  padding: 10px;
  border-radius: 5px;
  z-index: 9999;
}
button {
  display: block;
  margin-bottom: 5px;
  padding: 5px;
  width: 100%;
}
</style>
