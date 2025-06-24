<script setup>
import Button from './Button.vue';

defineProps({});
</script>

<template>
  <div class="menu">
    <div>
      <Button>
        <i class="bi bi-plus-lg"></i>
      </Button>
      <Button>❤️</Button>
      <Button>🔥</Button>
      <Button>😂</Button>
      <Button>
        <i class="bi bi-list"></i>
      </Button>
    </div>
    <div>
      <Button>
        <i class="bi bi-gear"></i>
      </Button>
      <Button>
        <i class="bi bi-person"></i>
      </Button>
    </div>
  </div>
</template>

<style scoped>
.menu {
  grid-column: span 1;
  width: 100%;
  height: 89%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
  border-radius: 8px;
}
.menu > div {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 8px;
}
</style>
