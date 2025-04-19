<template>
    <USelectMenu
      v-model="selectedColor"
      :options="colorOptions"
      option-attribute="label"
      icon="i-heroicons-swatch"
      class="w-48"
    >
      <template #option="{ option }">
        <div class="flex items-center gap-2">
          <span
            class="w-3 h-3 rounded-full"
            :class="colorSwatchClass(option.value)"
          />
          <span>{{ option.label }}</span>
        </div>
      </template>
    </USelectMenu>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted, watch } from 'vue'
  import { useNuxtApp } from '#app'
  
  const colorOptions = [
    { label: 'Blue', value: 'blue' },
    { label: 'Emerald', value: 'emerald' },
    { label: 'Rose', value: 'rose' },
    { label: 'Violet', value: 'violet' },
    { label: 'Fuchsia', value: 'fuchsia' },
    { label: 'Amber', value: 'amber' },
    { label: 'Teal', value: 'teal' },
  ]
  
  // Default color
  const defaultColor = 'blue'
  
  // Track selected color
  const selectedColor = ref(defaultColor)
  
  // Only interact with localStorage on the client-side (in `onMounted`)
  onMounted(() => {
    const { isClient } = useNuxtApp()
  
    if (isClient) {
      // Check if the color is saved in localStorage
      const savedColor = localStorage.getItem('primaryColor') || defaultColor
      selectedColor.value = savedColor
  
      // Dynamically apply the class for the selected color
      document.documentElement.classList.add(`ui-primary-${savedColor}`)
    }
  })
  
  // Watch for changes to the selected color and update localStorage
  watch(selectedColor, (newColor, oldColor) => {
  const { isClient } = useNuxtApp()

  if (isClient) {
    localStorage.setItem('primaryColor', newColor)

    // Remove old class and add new class
    if (oldColor) {
      document.documentElement.classList.remove(`ui-primary-${oldColor}`)
    }
    document.documentElement.classList.add(`ui-primary-${newColor}`)
  }
})

  
  // Swatch color classes — safe for Tailwind
  const colorSwatchClass = (color: string) => ({
    'bg-blue-500': color === 'blue',
    'bg-emerald-500': color === 'emerald',
    'bg-rose-500': color === 'rose',
    'bg-violet-500': color === 'violet',
    'bg-fuchsia-500': color === 'fuchsia',
    'bg-amber-500': color === 'amber',
    'bg-teal-500': color === 'teal',
  })
  </script>
  