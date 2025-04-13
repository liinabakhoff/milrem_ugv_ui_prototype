<template>
  <div class="waypoints-list">
    <h3>Saved Waypoints</h3>
    <ul>
      <li v-for="wp in waypoints" :key="wp.id">
        {{ wp.name }} ({{ wp.lat.toFixed(4) }}, {{ wp.lng.toFixed(4) }})
        <button @click="$emit('drive', wp.id)">Drive</button>
        <button @click="$emit('rename', wp.id)">Rename</button>
        <button @click="$emit('delete', wp.id)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  waypoints: { id: number; name: string; lat: number; lng: number }[]
}>()

const emit = defineEmits<{
  (e: 'drive', id: number): void
  (e: 'rename', id: number): void
  (e: 'delete', id: number): void
}>()
</script>

<style scoped>
.waypoints-list {
  position: absolute;
  top: 20px;
  left: 20px;
  background: white;
  padding: 10px;
  border: 1px solid black;
  border-radius: 5px;
  max-height: 300px;
  overflow-y: auto;
  z-index: 9999;
}

.waypoints-list ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.waypoints-list li {
  margin-bottom: 8px;
}

.waypoints-list button {
  margin-left: 5px;
}
</style>
