<template>
    <div v-if="icon" class="p-4 bg-primary-500 rounded-full inline-block">
        <UIcon :name="icon" class="w-6 h-6 text-white flex" />
    </div>

    <UAvatar
        v-else-if="img"
        :src="img"
        :alt="imgAlt"
        :size="imgSize || '2xl'"
    />
</template>

<script setup lang="ts">
const props = defineProps<{
    icon?: string
    img?: string
    imgAlt?: string
    imgSize?: '3xs' | '2xs' | 'xs' | 'sm' | 'md' | 'lg' | 'xl' | '2xl' | '3xl' | string
}>()
</script>