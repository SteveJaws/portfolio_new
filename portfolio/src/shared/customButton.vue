<template>
    <button @mouseenter="hover = true" @mouseleave="hover = false" @click="clicked">
       <span class="label">{{ buttonText }}</span>
       <div v-if="hover" class="fillament"></div>
    </button>

    <button v-if="!hover" class="button-border"><span>{{ buttonText }}</span></button>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const emit = defineEmits(["clicked"]);

const props = defineProps({
    title: String,
});

const buttonText = ref("");

const hover = ref(false);

function clicked(){
    emit("clicked");
}

onMounted(() => {
    buttonText.value = props.title
});
</script>

<style lang="scss" scoped>
@use "../assets/base.scss" as *;

button{
    border: none;
    border-radius: 1rem;
    position: relative;
    background: transparent;
    padding: 1rem 2rem;
    overflow: hidden;

    &:hover{
        cursor: pointer;
    }

    .label{
        position: relative;
        z-index: 1;
        color: white;
        background-color: transparent;
    }

    .fillament{
        width: 100%;
        height: 100%;
        border-radius: 1rem;
        position: absolute;
        left: 0;
        top: 0;
        background-color: $main-color;
        border-bottom-right-radius: 100%;

        animation: fill-width 0.5s ease-in,
        fill-heigth 0.5s ease-in,
        fill-radius 0.5s ease-in forwards,
        hover 1s ease-in-out infinite 0.5s;
    }
}

.button-border{
    padding: 1rem 2rem;
    position: absolute;
    top: -2px;
    left: -2px;
    background: transparent;
    pointer-events: none;
    border: 2px solid $secondary-color;
    animation: idle 2s ease-in-out infinite;
    
    span{
        opacity: 0;
    }
}

@keyframes fill-width{
    0%{
        width: 0;
    }
    100%{
        width: 100%;
    }
}

@keyframes fill-heigth{
    0%{
        height: 0;
    }
    100%{
        height: 100%;
    }
}

@keyframes fill-radius{
    0%{
        border-bottom-right-radius: 100%
    }
    80%{
        border-bottom-right-radius: 100%
    }
    100%{
        border-bottom-right-radius: 1rem;
    }
}

@keyframes hover{
    0%{
        transform: rotate(0) scale(1);
    }
    50%{
        transform: rotate(0) scale(0.9);
    }
    100%{
        transform: rotate(0) scale(1);
    }
}

@keyframes idle{
    0%{
        transform: scale(1);
    }
    50%{
        transform: scale(1.1);
    }
    100%{
        transform: scale(1);
    }
}
</style>