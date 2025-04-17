<template>
    <div class="*:h-1 *:bg-primary-500 *:rounded-full *:block space-y-1">
        <span class="w-8 animate-divider-scale" style="animation-delay: 0.3s"></span>
        <span class="w-20 animate-divider-scale" style="animation-delay: 0.1s"></span>
        <span class="w-16 animate-divider-scale" style="animation-delay: 0.2s"></span>
    </div>
</template>

<style scoped>
@keyframes scaleIn {
    0% {
        transform: scaleX(0);
        opacity: 0;
    }

    100% {
        transform: scaleX(1);
        opacity: 1;
    }
}

.animate-divider-scale {
    transform-origin: left;
    animation: scaleIn 0.4s ease-out forwards;
}
</style>