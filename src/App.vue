<template>
  <EngineButton :engine-on="engineOn" @toggle-engine="toggleEngine" />
  <MovementControl
    :engine-on="engineOn"
    :rename-modal-open="renameModal.visible"
    @engine-off-warning="showEngineOffWarning"
    @move="moveUGV"
  />
  <UgvMap
    :position="position"
    :waypoints="waypoints"
    @drive-to="driveToCoordinates"
    @save-waypoint="saveWaypoint"
  />
  <WaypointsList
    v-if="waypoints.length > 0"
    :waypoints="waypoints"
    @drive="driveToWaypoint"
    @rename="renameWaypoint"
    @delete="deleteWaypoint"
  />
  <Notification :visible="notification.visible" :message="notification.message" />
  <RenameModal
    v-if="renameModal.visible"
    :currentName="renameModal.currentName"
    @cancel="closeRenameModal"
    @confirm="confirmRename"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import EngineButton from './components/controls/EngineButton.vue'
import MovementControl from './components/controls/MovementControl.vue'
import UgvMap from './components/map/UgvMap.vue'
import WaypointsList from './components/waypoints/WaypointsList.vue'
import Notification from './components/ui/Notification.vue'
import RenameModal from './components/ui/RenameModal.vue'

const engineOn = ref(false)
const toggleEngine = () => {
  engineOn.value = !engineOn.value
}

const position = ref({ x: 24.8485589, y: 59.4296598 })

const moveUGV = (direction: 'up' | 'down' | 'left' | 'right') => {
  // Prevent UGV movement when rename modal is open
  if (renameModal.value.visible) return

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

const driveToCoordinates = (coords: { lat: number; lng: number }) => {
  if (!engineOn.value) {
    showEngineOffWarning()
    return
  }
  position.value = { x: coords.lng, y: coords.lat }
}

function driveToWaypoint(id: number) {
  if (!engineOn.value) {
    showEngineOffWarning()
    return
  }
  const waypoint = waypoints.value.find((wp) => wp.id === id)
  if (waypoint) {
    position.value = { x: waypoint.lng, y: waypoint.lat }
  }
}

function renameWaypoint(id: number) {
  openRenameModal(id)
}

function deleteWaypoint(id: number) {
  waypoints.value = waypoints.value.filter((wp) => wp.id !== id)
}

const notification = ref({
  visible: false,
  message: '',
})

function showNotification(message: string) {
  notification.value.message = message
  notification.value.visible = true

  setTimeout(() => {
    notification.value.visible = false
  }, 3000) // Hide after 3 seconds
}

const showEngineOffWarning = () => {
  showNotification('Engine is off. Please start the engine!')
}

const renameModal = ref({
  visible: false,
  waypointId: null as number | null,
  currentName: '',
})

function openRenameModal(id: number) {
  const waypoint = waypoints.value.find((wp) => wp.id === id)
  if (waypoint) {
    renameModal.value = {
      visible: true,
      waypointId: waypoint.id,
      currentName: waypoint.name,
    }
  }
}

function closeRenameModal() {
  renameModal.value.visible = false
}

function confirmRename(newName: string) {
  if (renameModal.value.waypointId !== null) {
    const waypoint = waypoints.value.find((wp) => wp.id === renameModal.value.waypointId)
    if (waypoint && newName.trim()) {
      waypoint.name = newName.trim()
    }
  }
  closeRenameModal()
}
</script>
