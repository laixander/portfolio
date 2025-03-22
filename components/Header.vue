<template>
    <header class="px-4 py-2.5 flex items-center justify-between border-b border-gray-200 dark:border-gray-800">
        <h1 class="text-primary-500 dark:text-primary-400 flex items-center gap-2">
            <UIcon :name="icon || 'ri-quill-pen-ai-line'" class="size-6" />
            {{ title || 'Hello' }}
        </h1>
        <ColorMode />
    </header>
</template>

<script setup lang="ts">
const props = defineProps<{
  icon?: string;
  title?: string;
}>();
</script>