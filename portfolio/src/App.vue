<template>
  <ViewLoader />
  asd
  <RouterView />
</template>

<script setup>
import { RouterView } from 'vue-router'
import { eventBus } from '@/utils/eventBus'
import ViewLoader from './shared/viewLoader.vue';

    eventBus.on("loadPage", (data) => {
        console.log(data);
    });
</script>