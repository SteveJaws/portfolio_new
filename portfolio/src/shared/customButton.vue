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
    color: white;

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
        width: 200%;
        height: 210%;
        border-radius: 1rem;
        position: absolute;
        left: -50%;
        top: -75%;
        background-color: $main-color;

        transform: rotate(30deg);

        animation: fill 0.5s ease-in forwards,
        transform 0.5s ease-in-out forwards 0.6s,
        hover 1s ease-in-out infinite 1.1s;
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

@keyframes fill{
    0%{
        left: -300%;
    }
    100%{
        left: -50%;
    }
}

@keyframes transform{
    0%{
        width: 200%;
        height: 210%;
        left: -50%;
        top: -75%;
        transform: rotate(30deg);
    }
    100%{
        width: 100%;
        height: 100%;
        left: 0%;
        top: 0%;
        transform: rotate(0);
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