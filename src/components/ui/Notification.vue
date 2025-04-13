<template>
  <transition name="fade-slide">
    <div v-if="visible" :class="['notification', type]">
      {{ message }}
    </div>
  </transition>
</template>

<script setup lang="ts">
const props = defineProps<{
  visible: boolean
  message: string
  type: 'info' | 'warning' | 'success'
}>()
</script>

<style scoped>
.notification {
  position: fixed;
  top: 10px;
  left: 50%;
  margin-left: -150px;
  padding: 12px 20px;
  border-radius: 8px;
  font-weight: bold;
  z-index: 9999;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  animation: fadeInOut 3s ease-in-out;
  background-color: #e67e22;
  color: white;
}

/* Types */
.notification.info {
  background-color: #4a90e2; /* Soft blue */
}

.notification.warning {
  background-color: #e67e22; /* Orange */
}

.notification.success {
  background-color: #27ae60; /* Green */
}

.fade-slide-enter-active,
.fade-slide-leave-active {
  transition:
    opacity 0.4s ease,
    transform 0.4s ease;
}

.fade-slide-enter-from,
.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

.fade-slide-enter-to,
.fade-slide-leave-from {
  opacity: 1;
  transform: translateY(0);
}
</style>
