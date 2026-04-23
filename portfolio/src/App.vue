<template>
  <Transition v-if="true" />
  <RouterView />
</template>

<script setup>
import { RouterView } from 'vue-router'
import { eventBus } from '@/utils/eventBus'
import Transition from './shared/transition.vue';

    eventBus.on("loadPage", (data) => {
        console.log(data);
    });
</script>