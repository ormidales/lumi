<script setup>
import Button from './Button.vue';

const emit = defineEmits(['changeView']);

const changeView = (view) => {
  emit('changeView', view);
};
</script>

<template>
  <div class="menu">
    <div>
      <Button @click="changeView('Types')">
        <i class="bi bi-plus-lg"></i>
      </Button>
      <Button @click="changeView('Chat')">❤️</Button>
      <Button @click="changeView('Chat')">🔥</Button>
      <Button @click="changeView('Chat')">😂</Button>
      <Button @click="changeView('List')">
        <i class="bi bi-list"></i>
      </Button>
    </div>
    <div>
      <Button @click="changeView('Settings')">
        <i class="bi bi-gear"></i>
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

.menu>div {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 8px;
}
</style>
