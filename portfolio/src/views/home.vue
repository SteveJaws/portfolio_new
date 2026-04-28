<template>
    <div class="container">
        <div class="title-card">
            <h1 v-motion :initial="{scale:0}" :animate="{scale:1}">A</h1>
            <h2 v-motion :initial="{scale:0}" :animate="{scale:1}">S</h2>
        </div>

        <div @mouseup="releaseZipper" @mousedown="holdZipper" id="zipper" class="zipper"></div>
    </div>
</template>

<script setup>
import { eventBus } from '@/utils/eventBus'

let mouseX = 0;
let mouseY = 0;
let screenWidth = window.innerWidth;
let screenHeight = window.innerHeight;

let following = false;

let zipper;

setTimeout(() => {
    zipper = document.getElementById("zipper");
}, 100);

document.addEventListener("mousemove", (event) => {
    mouseX = event.clientX;
    mouseY = event.clientY;
})

function holdZipper(){
    following = true;
    zipper.style.borderRadius = "50%"
    followLoop();
}

function releaseZipper(){
    following = false;
    zipper.style.borderRadius = "1rem"
}

function page(page){
    eventBus.emit('loadPage', page);
}

function followLoop(){
    if (!following) return;

    zipper.style.left = (mouseX / screenWidth * 100) + "%";
    zipper.style.top = (mouseY / screenHeight * 100) + "%";

    requestAnimationFrame(followLoop);
}
</script>

<style lang="scss" scoped>
@use "../assets/base.scss" as *;

.container{
    width: 100vw;
    height: 100vh;
    background-color: $background-color;

    .title-card{
        position: absolute;
        left: 50%;
        top: 20%;
        transform: translate(-50%, -20%);
        pointer-events: none;
    }
}

.zipper{
    width: 8rem;
    height: 8rem;
    border-radius: 0.2rem;
    position: absolute;
    left: 50%;
    top: 70%;
    transform: translate(-50%, -70%);
    background-color: white;
    animation: zipper-idle 1s linear infinite;
    transition: all 0.5s ease-in-out;

    &:hover{
        border-radius: 1rem;
        width: 9rem;
        height: 9rem;
    }
}

@keyframes zipper-idle{
    100%{
        transform: translate(-50%, -70%) rotate(90deg);
    }
}
</style>