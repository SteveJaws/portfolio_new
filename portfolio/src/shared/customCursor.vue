<template>
    <div id="cursor" class="cursor">
        <div v-if="scrolling" id="scrollIndicator" class="scroll-indicator"></div>
    </div>
</template>

<script setup>
import { eventBus } from '@/utils/eventBus';
import { onMounted, ref } from 'vue';

let cursor;

let invisible = true;

const scrolling = ref(false);

eventBus.on("hover", () => {
    console.log("hover");
});

onMounted(() => {
    cursor = document.getElementById("cursor");
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

document.addEventListener("mousedown", () => {
    makeClickRipple();
    cursor.style.width = "1.5rem";
    cursor.style.height = "1.5rem";
});

document.addEventListener("mouseup", () => {
    cursor.style.width = "1rem";
    cursor.style.height = "1rem";
});

let lastScrollY = window.scrollY;

document.addEventListener("scroll", () => {
    scrolling.value = true;

    const scrollIndicator = document.getElementById("scrollIndicator");
    const currentScrollY = window.scrollY;

    cursor.style.height = "1.5rem";
    cursor.style.width = "0.5rem";
    cursor.style.borderRadius = "0.2rem";

    if (currentScrollY > lastScrollY) {
        scrollDown(scrollIndicator);
    } else{
        scrollUp(scrollIndicator);
    }

    lastScrollY = currentScrollY;
});

function scrollUp(scrollIndicator){
    scrollIndicator.style.top = 0;
}

function scrollDown(scrollIndicator){
    scrollIndicator.style.top = "50%";
}

document.addEventListener("scrollend", () => {
    scrolling.value = false;

    cursor.style.height = "1rem";
    cursor.style.width = "1rem";
    cursor.style.borderRadius = "50%";
});

function makeClickRipple(){
    let ripple = document.createElement("div");

    let cursorElement = cursor.getBoundingClientRect();
    ripple.classList.add("ripple");
    ripple.style.left = cursorElement.x + cursorElement.width / 2 + "px";
    ripple.style.top = cursorElement.y + cursorElement.height / 2 + "px";
    document.body.appendChild(ripple);

    setTimeout(() => {
        ripple.remove();
    }, 1000)
}
</script>

<style lang="scss">
@use "../assets/base.scss" as *;

.cursor{
    width: 1rem;
    height: 1rem;
    border-radius: 50%;
    position: fixed;
    transform: translate(-50%, -50%);
    transition: opacity 0.5s ease-in-out, background-color 0.5s ease-in-out, width 0.5s ease-in-out, height 0.5s ease-in-out, border-radius 0.5s ease-in-out;
    opacity: 0;
    pointer-events: none;
    z-index: 999;
    overflow: hidden;
    background-color: white;

    .scroll-indicator{
        width: 100%;
        height: 50%;
        position: absolute;
        transition: all 0.5s ease-in-out;
        animation: indicator 1s ease-in-out infinite;
        background-color: $main-color;
    }
}

.ripple{
    width: 2rem;
    height: 2rem;
    border: 0.2rem solid $main-color;
    border-radius: 50%;
    position: fixed;
    transform: translate(-50%, -50%);
    opacity: 0;
    animation: ripple 1s ease-in-out;
    z-index: 999;
    pointer-events: none;
}

@keyframes ripple{
    0%{
        opacity: 1;
    }
    100%{
        width: 7rem;
        height: 7rem;
        opacity: 0;
    }
}

.make-clicked{
    animation: make-clicked 0.5s ease-in-out forwards;
}

@keyframes make-clicked{
    100%{
        background-color: $main-color;
    }
}

.make-unclicked{
    animation: make-unclicked 0.5s ease-in-out forwards;
}

@keyframes make-unclicked{
    100%{
        background-color: white;
    }
}

@keyframes indicator{
    50%{
        opacity: 0.5;
    }
}
</style>