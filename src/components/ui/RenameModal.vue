<template>
  <div class="modal-overlay">
    <div class="modal">
      <h3>Rename Waypoint</h3>
      <input ref="nameInput" v-model="newName" placeholder="Enter new name" spellcheck="false" />
      <div class="actions">
        <button @click="$emit('cancel')">Cancel</button>
        <button @click="$emit('confirm', newName)">Confirm</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, nextTick, onBeforeUnmount, computed } from 'vue'

const props = defineProps<{
  currentName: string
}>()

const emit = defineEmits<{
  (e: 'confirm', newName: string): void
  (e: 'cancel'): void
}>()

const newName = ref(props.currentName)
const nameInput = ref<HTMLInputElement | null>(null)
const isDisabled = computed(() => newName.value.trim() === '')
watch(
  () => props.currentName,
  (newVal) => {
    newName.value = newVal
  },
)

function handleKeydown(event: KeyboardEvent) {
  if (event.key === 'Escape') {
    emit('cancel')
  } else if (event.key === 'Enter' && !isDisabled.value) {
    emit('confirm', newName.value)
  }
}

// Focus when the component is mounted
onMounted(() => {
  window.addEventListener('keydown', handleKeydown)
  nextTick(() => {
    nameInput.value?.focus()
  })
})

onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal {
  padding: 20px;
  width: 300px;
  text-align: center;
  background: #ffffff;
  border: 1px solid #ddd;
  border-radius: 5px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.modal h3 {
  margin-bottom: 1rem;
}

.actions {
  margin-top: 15px;
}

.actions button {
  margin: 0 5px;
}
</style>
