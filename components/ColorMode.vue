<template>
	<ClientOnly>
		<UButton :icon="isDark ? 'i-ri-moon-fill' : 'i-ri-sun-fill'" color="gray" variant="ghost"
			aria-label="Theme" @click="isDark = !isDark" />
		<template #fallback>
			<div class="w-8 h-8" />
		</template>
	</ClientOnly>
</template>
<script setup lang="ts">
const colorMode = useColorMode()
const isDark = computed({
	get() {
		return colorMode.value === 'dark'
	},
	set() {
		colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
	}
})
</script>