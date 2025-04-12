<script setup lang="ts">
import { ref } from 'vue'
import EngineButton from './components/controls/EngineButton.vue'
import MovementControl from './components/controls/MovementControl.vue'

const engineOn = ref(false)
const toggleEngine = () => {
  engineOn.value = !engineOn.value
}

// Track UGV position (x, y on grid for now)
const position = ref({ x: 0, y: 0 }) // Create a reactive object that Vue can track

const moveUGV = (direction: 'up' | 'down' | 'left' | 'right') => {
  if (!engineOn.value) return

  switch (direction) {
    case 'up':
      position.value.y += 1
      break
    case 'down':
      position.value.y -= 1
      break
    case 'left':
      position.value.x -= 1
      break
    case 'right':
      position.value.x += 1
      break
  }

  console.log('UGV Position:', position.value)
}

const showEngineOffWarning = () => {
  alert('Engine is off. Please start the engine!')
}
</script>

<template>
  <EngineButton :engine-on="engineOn" @toggle-engine="toggleEngine" />
  <MovementControl
    :engine-on="engineOn"
    @engine-off-warning="showEngineOffWarning"
    @move="moveUGV"
  />
</template>
