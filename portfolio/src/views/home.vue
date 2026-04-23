<template>
    hello world

    <button @click="clicked = !clicked" v-motion :initial="{ scale: 0 }" :animate="{ scale: 1 }">Click me</button>
</template>

<script setup>
    import { ref } from 'vue';

    const clicked = ref(false);
</script>