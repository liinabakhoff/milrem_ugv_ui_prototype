<template>
  <EngineButton :engine-on="engineOn" @toggle-engine="toggleEngine" />
  <MovementControl
    :engine-on="engineOn"
    @engine-off-warning="showEngineOffWarning"
    @move="moveUGV"
  />
  <UgvMap :position="position" />
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

const showEngineOffWarning = () => {
  alert('Engine is off. Please start the engine!')
}
</script>
