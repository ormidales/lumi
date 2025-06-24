<script setup>
import Chat from './Chat.vue';
import Menu from './Menu.vue';

defineProps({});
</script>

<template>
  <div class="card">
    <Chat />
    <Menu />
  </div>
</template>

<style scoped>
.card {
  position: absolute;
  bottom: 100px;
  right: 100px;
  width: 500px;
  height: 600px;
  z-index: 7;
  padding: 8px;
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  grid-gap: 8px;
  background-color: rgba(50, 50, 50, 0.4);
  backdrop-filter: blur(10px);
  border-radius: 16px;
}
</style>
