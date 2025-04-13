<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import L from 'leaflet'
import ActionPopup from '../ui/ActionPopup.vue'

const waypointIcon = L.icon({
  iconUrl: 'https://cdn-icons-png.flaticon.com/512/684/684908.png',
  iconSize: [20, 20],
  iconAnchor: [10, 20],
  popupAnchor: [0, -20],
})

const savedMarkers: L.Marker[] = []

const props = defineProps<{
  position: { x: number; y: number }
  waypoints: { id: number; name: string; lat: number; lng: number }[]
}>()

const mapContainer = ref<HTMLDivElement | null>(null)
let map: L.Map
let marker: L.Marker

let longPressTimer: ReturnType<typeof setTimeout> | null = null

function onMapMouseDown(e: L.LeafletMouseEvent) {
  const { lat, lng } = e.latlng
  const containerPoint = map.latLngToContainerPoint(e.latlng)

  longPressTimer = setTimeout(() => {
    tempWaypoint.value = { lat, lng }
    popupPosition.value = { x: containerPoint.x, y: containerPoint.y }

    if (tempMarker) {
      map.removeLayer(tempMarker)
    }

    tempMarker = L.marker([lat, lng]).addTo(map)
  }, 600)
}

function onMapMouseUp() {
  if (longPressTimer) {
    clearTimeout(longPressTimer)
    longPressTimer = null
  }
}
const popupPosition = ref<{ x: number; y: number } | null>(null)

const tempWaypoint = ref<{ lat: number; lng: number } | null>(null)
let tempMarker: L.Marker | null = null

onMounted(() => {
  if (!mapContainer.value) return

  // Initialize map
  map = L.map(mapContainer.value, {
    center: [props.position.y, props.position.x],
    zoom: 20,
    keyboard: false,
    zoomControl: false,
  })
  // Add tile layer (OpenStreetMap tiles)
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors',
  }).addTo(map)

  // Add initial marker
  marker = L.marker([props.position.y, props.position.x]).addTo(map)

  map.on('mousedown', onMapMouseDown)
  map.on('mouseup', onMapMouseUp)
  map.on('mouseout', onMapMouseUp)
})

// Watch for position changes
watch(
  () => props.position,
  (newPos) => {
    if (marker) {
      marker.setLatLng([newPos.y, newPos.x])
      map.flyTo([newPos.y, newPos.x], map.getZoom())
    }
  },
  { deep: true },
)

const emit = defineEmits<{
  (e: 'drive-to', coords: { lat: number; lng: number }): void
  (e: 'save-waypoint', coords: { lat: number; lng: number }): void
}>()

function clearTempWaypoint() {
  tempWaypoint.value = null
  popupPosition.value = null
  if (tempMarker) {
    map.removeLayer(tempMarker)
    tempMarker = null
  }
}

watch(
  () => props.waypoints,
  (newWaypoints) => {
    if (!map) {
      return
    }

    savedMarkers.forEach((marker) => map.removeLayer(marker))
    savedMarkers.length = 0

    newWaypoints.forEach((wp) => {
      const marker = L.marker([wp.lat, wp.lng], { icon: waypointIcon })
        .addTo(map)
        .bindPopup(wp.name)

      savedMarkers.push(marker)
    })
  },
  { immediate: true, deep: true },
)

function handleDrive() {
  if (tempWaypoint.value) {
    emit('drive-to', tempWaypoint.value)
    clearTempWaypoint()
  }
}

function handleSave() {
  if (tempWaypoint.value) {
    emit('save-waypoint', tempWaypoint.value)
    clearTempWaypoint()
  }
}

function handleDiscard() {
  clearTempWaypoint()
}
</script>

<style scoped>
.map-container {
  height: 100vh;
  width: 100%;
}
</style>

<template>
  <div ref="mapContainer" class="map-container"></div>
  <ActionPopup
    v-if="tempWaypoint && popupPosition"
    :style="{
      position: 'absolute',
      left: popupPosition.x + 'px',
      top: popupPosition.y + 'px',
      zIndex: 9999,
    }"
    @drive="handleDrive"
    @save="handleSave"
    @discard="handleDiscard"
  />
</template>
