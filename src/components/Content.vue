<script setup>
import Chat from './Chat.vue';
import List from './List.vue';
</script>

<template>
    <div class="content">
        <Chat />
        <!-- <List /> -->
    </div>
</template>

<style scoped>
.content {
    grid-column: span 7;
    width: 100%;
    height: 100%;
}
</style>