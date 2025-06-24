<script setup>
import { ref } from 'vue';
import Ball from './Ball.vue';
import Card from './Card.vue';

const isCardVisible = ref(true);

const toggleCardVisibility = () => {
  isCardVisible.value = !isCardVisible.value;
};
</script>

<template>
  <Ball @click="toggleCardVisibility" />
  <Card :isVisible="isCardVisible" />
</template>
