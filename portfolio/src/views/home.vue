<template>
    <CustomButton title="button" @clicked="() => console.log('hello')" />
</template>

<script setup>
import CustomButton from '@/shared/customButton.vue';
import { eventBus } from '@/utils/eventBus'

function page(page){
    eventBus.emit('loadPage', page);
}
</script>