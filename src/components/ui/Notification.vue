<template>
  <div v-if="visible" class="notification">
    {{ message }}
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  visible: boolean
  message: string
}>()
</script>

<style scoped>
.notification {
  position: fixed;
  top: 10px;
  left: 50%;
  margin-left: -150px;
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 12px 20px;
  border-radius: 8px;
  font-weight: bold;
  z-index: 9999;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  animation: fadeInOut 3s ease-in-out;
}

@keyframes fadeInOut {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  10% {
    opacity: 1;
    transform: translateY(0);
  }
  90% {
    opacity: 1;
    transform: translateY(0);
  }
  100% {
    opacity: 0;
    transform: translateY(20px);
  }
}
</style>
