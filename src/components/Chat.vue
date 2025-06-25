<script setup>
import Button from './Button.vue';
</script>

<template>
  <div class="chat">
    <div class="header">
      <span class="documents">Documents</span>
      <small>/</small>
      Première conversation
    </div>
    <div class="body"></div>
    <div class="footer">
      <textarea>
Bonjour, je voudrais que tu me créer un document avec les informations suivantes ... </textarea>
      <Button>
        <i class="bi bi-send"></i>
      </Button>
    </div>
  </div>
</template>

<style scoped>
.chat {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 8px;
  border-radius: 8px;
}

.header {
  height: 15px;
  padding: 8px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 8px;
  background-color: rgba(50, 50, 50, 0.5);
  border-radius: 8px;
  font-size: 10px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.8);
}

.header>span {
  padding: 1px 2px;
  border-radius: 4px;
}

.header>span.documents {
  background-color: rgba(48, 137, 44, 1);
}

.header>small {
  font-size: 8px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.5);
}

.body {
  height: 475px;
  padding: 8px;
  background-color: rgba(50, 50, 50, 0.5);
  border-radius: 8px;
}

.footer {
  height: 50px;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-gap: 8px;
  border-radius: 8px;
}

.footer>textarea {
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
