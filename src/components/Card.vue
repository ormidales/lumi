<script setup>
import { ref, computed } from 'vue';
import Content from './Content.vue';
import Menu from './Menu.vue';

const props = defineProps({
  isVisible: {
    type: Boolean,
    default: true,
  },
});

const activeView = ref('Chat');

const cardClass = computed(() => ({
  'card': true,
  'hidden': !props.isVisible,
}));

const handleViewChange = (view) => {
  activeView.value = view;
};
</script>

<template>
  <div :class="cardClass">
    <Content :active-view="activeView" />
    <Menu @change-view="handleViewChange" />
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
  opacity: 1;
  transform: translateY(0);
  transition: all 0.3s ease-in-out;
}

.card.hidden {
  opacity: 0;
  pointer-events: none;
  transform: translateX(250px) translateY(300px) scale(0.05);
}
</style>
