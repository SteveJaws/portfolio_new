<template>
    <div id="container" class="container"></div>
</template>

<script setup>
import { eventBus } from '@/utils/eventBus';
import { onMounted } from 'vue';

let cursor;

let invisible = true;

let clicking = false;

eventBus.on("hover", () => {
    console.log("hover");
});

onMounted(() => {
    cursor = document.getElementById("container");
});

document.addEventListener("mousemove", (event) => {
    if(invisible){
        cursor.style.opacity = 1;
        invisible = false;
    }

    let posX = event.clientX;
    let posY = event.clientY;

    setTimeout(() => {
        cursor.style.left = posX + "px";
        cursor.style.top = posY + "px";
    }, 100)
});

document.addEventListener("click", () => {
    if(clicking) return;
    cursor.classList.add("click");
    clicking = true;
    makeClickRipple();
    setTimeout(() => {
        cursor.classList.remove("click");
        clicking = false;
    }, 500);
});

function makeClickRipple(){
    let ripple = document.createElement("div");
    ripple.classList.add("ripple")
    console.log("chekc");
    cursor.appendChild(ripple);
}
</script>

<style scoped lang="scss">
@use "../assets/base.scss" as *;

.container{
    width: 1rem;
    height: 1rem;
    border-radius: 50%;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    transition: opacity 0.5s ease-in-out;
    opacity: 0;
    pointer-events: none;
    z-index: 999;
    background-color: white;
}

.ripple{
    width: 2rem;
    height: 2rem;
    border: 0.2rem solid $main-color;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    opacity: 0;
    animation: ripple 0.5s ease-in-out;
}

.click{
    animation: click 0.5s ease-in-out;
}

@keyframes ripple{
    0%{
        opacity: 1;
    }
    100%{
        width: 5rem;
        height: 5rem;
        opacity: 0.2;
    }
}

@keyframes click{
    0%{
        transform: translate(-50%, -50%) scale(1);
    }
    50%{
        transform: translate(-50%, -50%) scale(2);
        background-color: $main-color;
    }
    100%{
        transform: translate(-50%, -50%) scale(1);
    }
}
</style>