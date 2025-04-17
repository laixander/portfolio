<template>
    <h3 v-if="title" :class="[
        'font-semibold text-gray-800 dark:text-gray-100 lg:mb-8',
        sizeClass,
        alignClass
    ]">
        <template v-if="hasHighlight">
            <span v-if="before">{{ before }}</span>
            <span
                class="before:block before:absolute before:-inset-1 before:-skew-y-3 before:bg-primary-500 relative inline-block mx-1 highlight-animate">
                <span class="relative text-white">{{ highlight }}</span>
            </span>
            <span v-if="after">{{ after }}</span>
        </template>
        <template v-else>
            {{ title }}
        </template>
    </h3>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
    title?: string
    highlight?: string
    align?: 'left' | 'center' | 'right'
    size?: string // Accept custom Tailwind size classes like 'text-3xl', 'text-5xl', etc.
}>()

const hasHighlight = computed(() => {
    return !!props.title && !!props.highlight && props.title.includes(props.highlight)
})

const before = computed(() => {
    if (!hasHighlight.value) return ''
    const index = props.title!.indexOf(props.highlight!)
    return props.title!.slice(0, index)
})

const after = computed(() => {
    if (!hasHighlight.value) return ''
    const index = props.title!.indexOf(props.highlight!)
    return props.title!.slice(index + props.highlight!.length)
})

const alignClass = computed(() => {
    switch (props.align) {
        case 'center':
            return 'text-center'
        case 'right':
            return 'text-right'
        default:
            return ''
    }
})

const sizeClass = computed(() => props.size ?? 'text-4xl')
</script>

<style scoped>
@keyframes glow {
    0%,
    100% {
        filter: brightness(1);
        transform: skewY(-3deg);
    }
    50% {
        filter: brightness(1.3);
        transform: skewY(-3deg) scale(1.02);
    }
}

.highlight-animate {
    animation: glow 2s ease-in-out infinite;
}
</style>