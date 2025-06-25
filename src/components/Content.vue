<script setup>
import Types from './Types.vue';
import Chat from './Chat.vue';
import List from './List.vue';
import Settings from './Settings.vue';
</script>

<template>
    <div class="content">
        <Types />
        <Chat />
        <List />
        <Settings />
    </div>
</template>

<style scoped>
.content {
    grid-column: span 7;
    width: 100%;
    height: 100%;
}
</style>