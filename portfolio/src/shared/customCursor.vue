<template>
    <div id="cursor" class="cursor"></div>
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
    cursor.style.backgroundColor = "#E54861";
});

document.addEventListener("mouseup", () => {
    cursor.style.backgroundColor = "white";
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
    position: absolute;
    transform: translate(-50%, -50%);
    transition: opacity 0.5s ease-in-out, background-color 0.5s ease-in-out;
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
    position: absolute;
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
</style>