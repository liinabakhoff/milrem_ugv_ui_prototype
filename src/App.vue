<template>
  <EngineButton :engine-on="engineOn" @toggle-engine="toggleEngine" />
  <MovementControl
    :engine-on="engineOn"
    @engine-off-warning="showEngineOffWarning"
    @move="moveUGV"
  />
  <UgvMap
    :position="position"
    :waypoints="waypoints"
    @drive-to="driveToWaypoint"
    @save-waypoint="saveWaypoint"
  />
  <ul>
    <li v-for="wp in waypoints" :key="wp.id">{{ wp.name }}: ({{ wp.lat }}, {{ wp.lng }})</li>
  </ul>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import EngineButton from './components/controls/EngineButton.vue'
import MovementControl from './components/controls/MovementControl.vue'
import UgvMap from './components/map/UgvMap.vue'

const engineOn = ref(false)
const toggleEngine = () => {
  engineOn.value = !engineOn.value
}

const position = ref({ x: 24.8485589, y: 59.4296598 })

const moveUGV = (direction: 'up' | 'down' | 'left' | 'right') => {
  if (!engineOn.value) return
  const step = 0.00001
  switch (direction) {
    case 'up':
      position.value.y += step
      break
    case 'down':
      position.value.y -= step
      break
    case 'left':
      position.value.x -= step
      break
    case 'right':
      position.value.x += step
      break
  }
}

const waypoints = ref<{ id: number; name: string; lat: number; lng: number }[]>([])

const saveWaypoint = (coords: { lat: number; lng: number }) => {
  waypoints.value.push({
    id: Date.now(), // Simple unique ID
    name: `Waypoint ${waypoints.value.length + 1}`,
    lat: coords.lat,
    lng: coords.lng,
  })
}

const driveToWaypoint = (coords: { lat: number; lng: number }) => {
  position.value = { x: coords.lng, y: coords.lat }
}

const showEngineOffWarning = () => {
  alert('Engine is off. Please start the engine!')
}
</script>
