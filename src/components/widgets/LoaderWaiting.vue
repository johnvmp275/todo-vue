<template>
    <div class="loader" v-if="!loaderIsActive">
        <span class="loader__element"></span>
        <span class="loader__element"></span>
        <span class="loader__element"></span>
    </div>
</template>

<script>
export default {
    props: {
        loaderIsActive: String
    }
}
</script>

<style scoped>
.loader {
    position: fixed;
    top: 0;
    left: 0;
    background: var(--background-gray-600);
    width: 100vw;
    height: 100vh;
    z-index: 9999999;
    display: flex;
    gap: 40px;
    justify-content: center;
    align-items: center;
}

.loader__element {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: var(--background-orange);
}

.loader__element:nth-child(1) {
    animation: preloader .6s ease-in-out alternate infinite;
}

.loader__element:nth-child(2) {
    animation: preloader .6s ease-in-out alternate .2s infinite;
}

.loader__element:nth-child(3) {
    animation: preloader .6s ease-in-out alternate .4s infinite;
}

@keyframes preloader {
    100% {
        transform: scale(2);
    }
}

</style>