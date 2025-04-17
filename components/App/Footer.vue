<template>
    <footer :class="footerConfig">
        <UContainer :ui="uiContainerConfig">
            <Feature
                v-for="(feature, index) in brand" 
                :key="index" 
                v-bind="feature"
            />
            <List :items="detailList" />
            <div class="space-y-2">
                <h4 class="font-medium text-gray-800 dark:text-gray-100">
                    Sitemap
                </h4>
                <UVerticalNavigation :links="links" :ui="uiNavConfig" />
            </div>
        </UContainer>
    </footer>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { brand, detailList, links } from '@/data/footer'

const props = defineProps<{
  gradient?: boolean
}>()

const footerConfig = computed(() => {
  const base = 'py-4 lg:py-20'
  const gradient = 'bg-gradient-to-t from-primary-100 to-white dark:from-primary-950'
  return props.gradient ? `${base} ${gradient}` : base
})

const uiContainerConfig = {
    base: 'grid grid-cols-1 lg:grid-cols-3 gap-8'
}

const uiNavConfig = {
    wrapper: 'border-s border-gray-200 dark:border-white/5',
    base: 'group block border-s -ms-px leading-6 before:hidden',
    padding: 'p-0 ps-4',
    rounded: '',
    font: '',
    ring: '',
    active: 'text-primary-500 dark:text-primary-400 border-current',
    inactive: 'border-transparent hover:border-gray-400 dark:hover:border-gray-500 text-gray-700 hover:text-gray-900 dark:text-gray-400 dark:hover:text-gray-300'
}
</script>

<style scoped>
:deep(ul) {
    @apply space-y-2.5;
}
</style>