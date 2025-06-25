<script setup>
import Types from './Types.vue';
import Chat from './Chat.vue';
import List from './List.vue';
import Settings from './Settings.vue';

const props = defineProps({
    activeView: {
        type: String,
        required: true,
        default: 'Chat'
    }
});
</script>

<template>
    <div class="content">
        <Types v-if="activeView === 'Types'" />
        <Chat v-if="activeView === 'Chat'" />
        <List v-if="activeView === 'List'" />
        <Settings v-if="activeView === 'Settings'" />
    </div>
</template>

<style scoped>
.content {
    grid-column: span 7;
    width: 100%;
    height: 100%;
}
</style>
