<script setup lang="ts">
import { ref } from 'vue'
import EngineButton from './components/controls/EngineButton.vue'
import MovementControl from './components/controls/MovementControl.vue'

const engineOn = ref(false)
const toggleEngine = () => {
  engineOn.value = !engineOn.value
}

const showEngineOffWarning = () => {
  alert('Engine is off. Please start the engine!')
}
</script>

<template>
  <EngineButton :engine-on="engineOn" @toggle-engine="toggleEngine" />
  <MovementControl :engine-on="engineOn" @engine-off-warning="showEngineOffWarning" />
</template>
