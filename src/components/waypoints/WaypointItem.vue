<template>
  <li>
    {{ waypoint.name }} ({{ toMgrs(waypoint.lat, waypoint.lng) }})
    <button @click="$emit('drive', waypoint.id)">Drive</button>
    <button @click="$emit('rename', waypoint.id)">Rename</button>
    <button @click="$emit('delete', waypoint.id)">Delete</button>
  </li>
</template>

<script setup lang="ts">
import * as mgrs from 'mgrs'
const props = defineProps<{
  waypoint: { id: number; name: string; lat: number; lng: number }
}>()

const emit = defineEmits<{
  (e: 'drive', id: number): void
  (e: 'rename', id: number): void
  (e: 'delete', id: number): void
}>()

function toMgrs(lat: number, lng: number): string {
  return mgrs.forward([lng, lat], 5) // 5 is the precision (adjustable!)
}
</script>

<style scoped>
button {
  margin-left: 5px;
}

li {
  margin-bottom: 8px;
  padding: 4px;
  border-radius: 4px;
  transition: background-color 0.2s ease;
}

li:hover {
  background-color: #eef3fb;
}
</style>
