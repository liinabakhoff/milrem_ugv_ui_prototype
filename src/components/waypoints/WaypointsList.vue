<template>
  <div class="waypoints-list">
    <h3>Saved Waypoints</h3>
    <ul>
      <WaypointItem
        v-for="wp in waypoints"
        :key="wp.id"
        :waypoint="wp"
        @drive="$emit('drive', wp.id)"
        @rename="$emit('rename', wp.id)"
        @delete="$emit('delete', wp.id)"
      />
    </ul>
  </div>
</template>

<script setup lang="ts">
import WaypointItem from './WaypointItem.vue'

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
  padding: 10px;
  border: 1px solid black;
  max-height: 300px;
  overflow-y: auto;
  z-index: 9999;
  background: rgba(255, 255, 255, 0.8);
  border: 1px solid #ddd;
  border-radius: 5px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.waypoints-list h3 {
  margin-bottom: 0.5rem;
}

.waypoints-list ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
</style>
