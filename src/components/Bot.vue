<script setup>
import { ref } from 'vue';
import Ball from './Ball.vue';
import Card from './Card.vue';

const isCardVisible = ref(false);
const isCardHovered = ref(false);

const toggleCardVisibility = () => {
  isCardVisible.value = !isCardVisible.value;
};
</script>

<template>
  <Ball @click="toggleCardVisibility" :is-card-hovered="isCardHovered" />
  <Card :isVisible="isCardVisible" @mouseenter="isCardHovered = true" @mouseleave="isCardHovered = false" />
</template>
