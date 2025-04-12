<script setup lang="ts">
import { ref } from 'vue'
import EngineButton from './components/controls/EngineButton.vue'

const engineOn = ref(false)
const toggleEngine = () => {
  engineOn.value = !engineOn.value
}
</script>

<template>
  <EngineButton :engineOn="engineOn" @toggleEngine="toggleEngine" />
</template>
