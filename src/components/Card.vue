<script setup>
import { computed } from 'vue';
import Content from './Content.vue';
import Menu from './Menu.vue';

const props = defineProps({
  isVisible: {
    type: Boolean,
    default: true,
  },
});

const cardClass = computed(() => ({
  'card': true,
  'hidden': !props.isVisible,
}));
</script>

<template>
  <div :class="cardClass">
    <Content />
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
  transition: all 0.3s ease-in-out;
  opacity: 1;
  transform: translateY(0);
}

.card.hidden {
  opacity: 0;
  pointer-events: none;
  transform: translateX(20px) translateY(20px);
}
</style>
