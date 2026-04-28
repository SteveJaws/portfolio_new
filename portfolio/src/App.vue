<template>
  <ViewLoader v-if="loadPage" :page="page" @loadedPage="loadedPage" />
  <RouterView />
</template>

<script setup>
import { ref } from 'vue';
import { RouterView } from 'vue-router'
import { eventBus } from '@/utils/eventBus'
import ViewLoader from './shared/viewLoader.vue';

  const loadPage = ref(false);
  const page = ref("");

  eventBus.on("loadPage", (data) => {
    page.value = data;
    loadPage.value = true;
  });

  function loadedPage(){
    loadPage.value = false;
    page.value = "";
  }
</script>