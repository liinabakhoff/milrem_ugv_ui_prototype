<template>
  <div class="movement-control">
    <p>Use your keyboard arrows to control the UGV.</p>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, defineProps, defineEmits } from 'vue'

const props = defineProps<{
  engineOn: boolean
}>()

const emit = defineEmits<{
  (event: 'engine-off-warning'): void
  (event: 'move', direction: 'up' | 'down' | 'left' | 'right'): void
}>()

function handleKeyDown(event: KeyboardEvent) {
  if (!props.engineOn) {
    emit('engine-off-warning')
    return
  }

  switch (event.key) {
    case 'ArrowUp':
      emit('move', 'up')
      console.log('Move Forward')
      break
    case 'ArrowDown':
      emit('move', 'down')
      console.log('Move Backward')
      break
    case 'ArrowLeft':
      emit('move', 'left')
      console.log('Move Left')
      break
    case 'ArrowRight':
      emit('move', 'right')
      console.log('Move Right')
      break
    default:
      break
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyDown)
})
</script>

<style scoped>
.movement-control {
  position: absolute;
  bottom: 20px;
  left: 20px;
  color: white;
  background-color: rgba(0, 0, 0, 0.4);
  padding: 10px;
  border-radius: 8px;
  font-size: 14px;
}
</style>
