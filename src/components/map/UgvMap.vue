<template>
  <div ref="mapContainer" class="map-container"></div>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import L from 'leaflet'

const props = defineProps<{
  position: { x: number; y: number }
}>()

const mapContainer = ref<HTMLDivElement | null>(null)
let map: L.Map
let marker: L.Marker

onMounted(() => {
  if (!mapContainer.value) return

  // Initialize map
  map = L.map(mapContainer.value, {
    center: [props.position.y, props.position.x],
    zoom: 20,
    keyboard: false,
  })
  // Add tile layer (OpenStreetMap tiles)
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors',
  }).addTo(map)

  // Add initial marker
  marker = L.marker([props.position.y, props.position.x]).addTo(map)
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
</script>

<style scoped>
.map-container {
  height: 100vh;
  width: 100%;
}
</style>
