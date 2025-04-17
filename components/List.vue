<template>
    <div class="space-y-6">
        <div v-for="(item, index) in items" :key="index" class="flex gap-4">
            <div class="py-0.5">
                <UIcon :name="item.icon" class="text-primary-500 w-5 h-5" />
            </div>
            <div class="space-y-1">
                <h4 class="font-medium text-gray-800 dark:text-gray-100">
                    {{ item.title }}
                </h4>
                <p class="font-light text-sm text-gray-500 dark:text-gray-400 mt-0">
                    {{ item.description }}
                </p>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
defineProps<{
    items: {
        icon: string
        title: string
        description: string
    }[]
}>()
</script>