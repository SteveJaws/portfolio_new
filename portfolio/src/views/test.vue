<template>
welcome to the test page
<button @click="loadPage">home page</button>
</template>

<script setup>
import { eventBus } from '@/utils/eventBus';

function loadPage(){
    eventBus.emit("loadPage", "/")
}
</script>