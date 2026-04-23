<template>
    <div class="container">
        <div class="arrow"></div>
        <span class="text">Loading</span>
    </div>
</template>

<style lang="scss">
    @use '../assets/base.scss' as *;

.container{
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: -100%;
    background-color: $main-color;

    animation:
      slide-down 1s ease-in-out forwards,
      slide-up 0.2s ease-in-out forwards 3.9s; 
}

.arrow{
    width: 10rem;
    height: 10rem;
    border-radius: 1rem;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: $tertiary-color;

    animation: make-arrow 0.5s ease-in-out forwards 1s,
    ready-arrow 0.5s ease-in-out forwards 1.6s,
    pulse 1s ease-in forwards 2.4s,
    fire-arrow 0.2s ease-in forwards 3.6s;

    z-index: 2;
}

.text{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: x-large;
    color: $tertiary-color;

    animation: breathe 1.5s ease-in-out infinite;

    z-index: 1;
}

@keyframes breathe{
    0%{
        opacity: 1;
    }
    50%{
        opacity: 0.5;
    }
    100%{
        opacity: 1;
    }
}

@keyframes slide-down{
  0%{ top: -100%; }
  100%{ top: 0; }
}

@keyframes slide-up{
  0%{ top: 0; }
  100%{ top: -100%; }
}

@keyframes make-arrow{
    0%{
        width: 10rem;
    }
    100%{
        width: 5rem;
    }
}

@keyframes ready-arrow{
    0%{
        top: 50%;
        transform: translate(-50%, -50%);
    }
    100%{
        top: 80%;
        transform: translate(-50%, -50%) rotate(360deg);
        background-color: $secondary-color;
    }
}

@keyframes pulse{
    0%{
        width: 5rem;
        background-color: $secondary-color;
    }
    30%{
        width: 4rem;
        background-color: $tertiary-color;
    }
    60%{
        width: 5rem;
        background-color: $secondary-color;
    }
    75%{
        width: 4rem;
        background-color: $tertiary-color;
    }
    90%{
        width: 5rem;
        background-color: $secondary-color;
    }
    93%{
        width: 4rem;
        background-color: $tertiary-color;
    }
    96%{
        width: 5rem;
        background-color: $secondary-color;
    }
    100%{
        width: 4rem;
        background-color: $tertiary-color;
    }
}

@keyframes fire-arrow{
    0%{
        top: 80%;
    }
    100%{
        top: -100%;
    }
}
</style>