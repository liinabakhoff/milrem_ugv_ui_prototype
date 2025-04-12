<template>
  <button @click="$emit('toggleEngine')" class="engine-button">
    <span class="icon">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        :stroke="iconColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <path d="M18.36 6.64a9 9 0 1 1-12.73 0"></path>
        <line x1="12" y1="2" x2="12" y2="12"></line>
      </svg>
    </span>
    {{ engineOn ? 'Stop Engine' : 'Start Engine' }}
  </button>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, computed } from 'vue'

const props = defineProps({
  engineOn: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits<{ (e: 'toggleEngine'): void }>()

const iconColor = computed(() => (props.engineOn ? '#f44336' : '#4CAF50'))
</script>

<style scoped>
.engine-button {
  position: absolute;
  top: 20px;
  right: 20px;
  background-color: rgba(128, 128, 128, 0.8);
  border: 2px solid black;
  color: white;
  padding: 10px 20px;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: background-color 0.3s;
  z-index: 999;
}

.engine-button:hover {
  background-color: rgba(100, 100, 100, 0.8);
}

.icon {
  display: flex;
  align-items: center;
}
</style>
