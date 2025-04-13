<template>
  <div class="modal-overlay">
    <div class="modal">
      <h3>Rename Waypoint</h3>
      <input ref="nameInput" v-model="newName" placeholder="Enter new name" />
      <div class="actions">
        <button @click="$emit('cancel')">Cancel</button>
        <button @click="$emit('confirm', newName)">Confirm</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, nextTick } from 'vue'

const props = defineProps<{
  currentName: string
}>()

const emit = defineEmits<{
  (e: 'confirm', newName: string): void
  (e: 'cancel'): void
}>()

const newName = ref(props.currentName)
const nameInput = ref<HTMLInputElement | null>(null)

watch(
  () => props.currentName,
  (newVal) => {
    newName.value = newVal
  },
)

// Focus when the component is mounted
onMounted(() => {
  nextTick(() => {
    nameInput.value?.focus()
  })
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
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 300px;
  text-align: center;
}

.actions {
  margin-top: 15px;
}

.actions button {
  margin: 0 5px;
}
</style>
