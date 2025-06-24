<script setup>
defineProps({});
</script>

<template>
  <button class="button">
    <slot />
  </button>
</template>

<style scoped>
.button {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(50, 50, 50, 0.5);
  border-radius: 8px;
  font-size: 24px;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}
.button > svg {
  width: 28px;
  height: 28px;
  color: rgba(255, 255, 255, 1);
}
.button:hover {
  transform: scale(1.1);
}
.button:active {
  transform: scale(0.9);
}
</style>
