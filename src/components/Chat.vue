<script setup>
import Button from './Button.vue';

defineProps({});
</script>

<template>
  <div class="chat">
    <div class="header">
      <span class="documents">Documents</span>
      <small>/</small>
      <span>Première conversation</span>
    </div>
    <div class="body"></div>
    <div class="footer">
      <textarea>
Bonjour, je voudrais que tu me créer un document avec les informations suivantes ... </textarea
      >
      <Button>
        <i class="bi bi-send"></i>
      </Button>
    </div>
  </div>
</template>

<style scoped>
.chat {
  grid-column: span 7;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 8px;
  border-radius: 8px;
}
.header {
  height: 2%;
  padding: 8px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 4px;
  background-color: rgba(50, 50, 50, 0.5);
  border-radius: 8px;
}
.header > span {
  margin: 0;
  font-size: 10px;
  color: rgba(255, 255, 255, 0.8);
}
.header > span.documents {
  color: rgba(70, 220, 40, 1) !important;
  font-weight: 600;
}
.header > small {
  margin: 0;
  font-size: 8px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.5);
}
.body {
  height: 80%;
  padding: 8px;
  background-color: rgba(50, 50, 50, 0.5);
  border-radius: 8px;
}
.footer {
  height: 10%;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-gap: 8px;
  border-radius: 8px;
}
.footer > textarea {
  grid-column: span 6;
  padding: 8px;
  background-color: rgba(50, 50, 50, 0.5);
  border: 0;
  border-radius: 8px;
  font-size: 12px;
  color: rgba(255, 255, 255, 1);
  resize: none;
}
</style>
