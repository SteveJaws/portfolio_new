<template>
    <div id="container" class="container">
        <div class="title-card">
            <h1 v-motion :initial="{scale:0}" :animate="{scale:1}">Aron van Duijn</h1>
            <h2 v-motion :initial="{scale:0}" :animate="{scale:1}">Software Developer</h2>
        </div>

        <span id="instruction" class="instruction">Hold me!</span>
        <div @mouseup="releaseZipper" @mousedown="holdZipper" id="zipper" class="zipper zipper-rotate"></div>
    </div>
</template>

<script setup>
import { eventBus } from '@/utils/eventBus'
import { onMounted } from 'vue';

let screenWidth;
let screenHeight;

let zipperStartWidth;
let zipperStartHeight;

let grow = false;

let zipper;

onMounted(() => {
    zipper = document.getElementById("zipper");

    zipper.addEventListener("touchstart", () => {
        zipper.style.backgroundColor = "#EFD5B7";
        holdZipper();
    });

    zipper.addEventListener("touchend", () => {
        zipper.style.backgroundColor = "white";
        releaseZipper();
    });

    zipperStartWidth = zipper.getBoundingClientRect().width;
    zipperStartHeight = zipper.getBoundingClientRect().height;

    let container = document.getElementById("container");

    screenWidth = container.getBoundingClientRect().width;
    screenHeight = container.getBoundingClientRect().height;
});

function holdZipper(){
    document.getElementById("instruction").style.opacity = 0;
    zipper.classList.remove("zipper-rotate");
    zipper.style.borderRadius = "50%";
    grow = true;
    growLoop();
}

function releaseZipper(){
    document.getElementById("instruction").style.opacity = 1;
    zipper.style.borderRadius = "1rem";
    grow = false;
    shrinkLoop();
}

function shrinkLoop(){
    if (grow) return;
    let currentWidth = zipper.getBoundingClientRect().width;
    let currentHeigth = zipper.getBoundingClientRect().width;

    if(currentWidth > zipperStartWidth || currentHeigth > zipperStartHeight){
        currentWidth -= 7 * screenWidth / screenHeight;
        currentHeigth -= 7 * screenWidth / screenHeight;

        zipper.style.width = currentWidth + "px";
        zipper.style.height = currentHeigth + "px";

        requestAnimationFrame(shrinkLoop);
    }
    else{
        zipper.classList.add("zipper-rotate");
    }
}

function growLoop(){
    if (!grow) return;

    let currentWidth = zipper.getBoundingClientRect().width;
    let currentHeigth = zipper.getBoundingClientRect().width;

    if(currentWidth > screenWidth * 1.2 && currentHeigth > screenHeight * 1.2){
        page("/test");
    }

    currentWidth += 7 * screenWidth / screenHeight;
    currentHeigth += 7 * screenWidth / screenHeight;

    zipper.style.width = currentWidth + "px";
    zipper.style.height = currentHeigth + "px";
    
    requestAnimationFrame(growLoop);
}

function page(page){
    eventBus.emit('loadPage', page);
}
</script>

<style lang="scss" scoped>
@use "../assets/base.scss" as *;

.container{
    width: 100vw;
    height: 100vh;
    background-color: $background-color;
    overflow: hidden;
    text-align: center;

    .title-card{
        position: absolute;
        left: 50%;
        top: 20%;
        transform: translate(-50%, -20%);
        pointer-events: none;
    }
}

.zipper{
    width: 5rem;
    height: 5rem;
    border-radius: 1rem;
    position: absolute;
    left: 50%;
    top: 70%;
    transform: translate(-50%, -70%);
    background-color: white;
    transition: border-radius 0.5s ease-in-out, background-color 0.5s ease-in-out, opacity 0.5s ease-in-out;
    z-index: 2;

    &:hover{
        cursor: pointer;
        background-color: $tertiary-color;
    }
}

.zipper-rotate{
    animation: zipper-rotate 1s linear infinite;
}

.instruction{
    position: absolute;
    top: 80%;
    left: 50%;
    transform: translate(-50%, -80%);
    transition: all 0.2s ease-in-out;
    z-index: 1;
}

@keyframes zipper-rotate{
    100%{
        transform: translate(-50%, -70%) rotate(90deg);
    }
}
</style>