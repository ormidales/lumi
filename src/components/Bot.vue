<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import Ball from './Ball.vue';
import Card from './Card.vue';

const isCardVisible = ref(false);
const isCardHovered = ref(false);

const ballRef = ref(null);
const cardRef = ref(null);

const toggleCardVisibility = () => {
  isCardVisible.value = !isCardVisible.value;
};

const openCard = () => {
  isCardVisible.value = true;
};

const handleClickOutside = (event) => {
  if (!isCardVisible.value) return;

  const ballEl = ballRef.value && ballRef.value.$el;
  const cardEl = cardRef.value && cardRef.value.$el;

  const clickedOnBall = ballEl && ballEl.contains(event.target);
  const clickedOnCard = cardEl && cardEl.contains(event.target);

  if (!clickedOnBall && !clickedOnCard) {
    isCardVisible.value = false;
  }
};

onMounted(() => {
  document.addEventListener('click', handleClickOutside, true);
});

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside, true);
});
</script>

<template>
  <Ball ref="ballRef" @click="toggleCardVisibility" @eye-click="openCard" :is-card-hovered="isCardHovered"
    :is-card-visible="isCardVisible" />
  <Card ref="cardRef" :isVisible="isCardVisible" @mouseenter="isCardHovered = true"
    @mouseleave="isCardHovered = false" />
</template>