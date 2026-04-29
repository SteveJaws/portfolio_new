<template>
Welcome to the test page
</template>

<script setup>
import { eventBus } from '@/utils/eventBus';

function loadPage(){
    eventBus.emit("loadPage", "/")
}
</script>