<template>
    <button @click="page('/test')">click me</button>
</template>

<script setup>
import { eventBus } from '@/utils/eventBus'

function page(page){
    eventBus.emit('loadPage', page);
}
</script>