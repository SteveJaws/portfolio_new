<template>
    <div class="top">
        <p>Welcome to the test page</p>
    </div>
    <div class="bottom">
        <p>welcome to bottom div</p>
    </div>
</template>

<style scoped lang="scss">
p{
    margin: 0;
}

.top{
    width: 100vw;
    height: 100vh;
    background: blue;
}

.bottom{
    width: 100vw;
    height: 100vh;
    background-color: green;
}
</style>

<script setup>
import { eventBus } from '@/utils/eventBus';

function loadPage(){
    eventBus.emit("loadPage", "/")
}
</script>