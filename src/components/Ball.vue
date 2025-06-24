<script setup>
defineProps({});
</script>

<template>
  <div class="ball">✨</div>
</template>

<style scoped>
.ball {
  position: absolute;
  bottom: 60px;
  right: 60px;
  width: 100px;
  height: 100px;
  z-index: 8;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(50, 50, 50, 0.5);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  font-size: 48px;
  cursor: pointer;
  transition: all 0.1s ease-in-out;
}
.ball:hover {
  transform: scale(1.1);
}
.ball:active {
  transform: scale(0.9);
}
</style>
