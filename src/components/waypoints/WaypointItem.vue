<template>
  <li>
    {{ waypoint.name }} ({{ waypoint.lat.toFixed(4) }}, {{ waypoint.lng.toFixed(4) }})
    <button @click="$emit('drive', waypoint.id)">Drive</button>
    <button @click="$emit('rename', waypoint.id)">Rename</button>
    <button @click="$emit('delete', waypoint.id)">Delete</button>
  </li>
</template>

<script setup lang="ts">
const props = defineProps<{
  waypoint: { id: number; name: string; lat: number; lng: number }
}>()

const emit = defineEmits<{
  (e: 'drive', id: number): void
  (e: 'rename', id: number): void
  (e: 'delete', id: number): void
}>()
</script>

<style scoped>
li {
  margin-bottom: 8px;
}

button {
  margin-left: 5px;
}
</style>
