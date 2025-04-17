<template>
    <div :class="['flex flex-col items-start gap-4 lg:gap-6', alignmentClass]">
        <FancyIcon :icon="icon" :img="img" :imgAlt="imgAlt" :imgSize="imgSize" />
        <div :class="['prose max-w-none mb-auto', alignmentClass]">
            <h4 v-if="title" :class="titleClass">
                {{ title }}
            </h4>
            <p v-if="description" :class="descriptionClass">
                <template v-for="(chunk, i) in parsedDescription" :key="i">
                    <template v-if="typeof chunk === 'string'">
                        {{ chunk }}
                    </template>
                    <template v-else-if="chunk.bold">
                        <strong class="font-semibold text-gray-700 dark:text-gray-300">{{ chunk.bold }}</strong>
                    </template>
                    <template v-else-if="chunk.italic">
                        <em class="italic text-gray-600 dark:text-gray-400">{{ chunk.italic }}</em>
                    </template>
                    <template v-else-if="chunk.link">
                        <a :href="chunk.link.href" class="text-primary-600 hover:underline dark:text-primary-400">
                            {{ chunk.link.text }}
                        </a>
                    </template>
                </template>
            </p>
        </div>
        <UButton v-if="button" variant="link" :label="button" icon="i-lucide-arrow-right" :trailing="true"
            :padded="false" @click="$emit('cta')" />
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
    icon?: string
    img?: string
    imgAlt?: string
    imgSize?: string
    title?: string
    titleColor?: string // e.g., 'text-gray-800'
    titleSize?: string // e.g., 'text-xl'
    description?: string
    descriptionSize?: string
    button?: string
    align?: 'left' | 'center' | 'right'
}>()

defineEmits<{
  (e: 'cta'): void
}>()

const titleClass = computed(() => {
  return `${props.titleSize ?? 'text-xl'} ${props.titleColor ?? 'text-gray-800 dark:text-gray-100'} font-semibold`
})

const descriptionClass = computed(() => {
  return `${props.descriptionSize ?? 'text-base'} font-light text-gray-500 dark:text-gray-400 text-pretty`
})

const alignmentClass = computed(() => {
  switch (props.align) {
    case 'center':
      return 'items-center text-center'
    case 'right':
      return 'items-end text-right'
    default:
      return 'items-start text-left'
  }
})

// Markdown-style parser for description string
const parsedDescription = computed(() => {
  if (!props.description) return []

  const str = props.description

  const tokens: (string | { bold?: string; italic?: string; link?: { text: string; href: string } })[] = []

  const pattern =
    /(\*\*(.*?)\*\*)|(_(.*?)_)|(`(.*?)`)|(\[(.*?)\]\((.*?)\))/g

  let lastIndex = 0
  let match

  while ((match = pattern.exec(str)) !== null) {
    if (match.index > lastIndex) {
      tokens.push(str.slice(lastIndex, match.index))
    }

    if (match[1]) tokens.push({ bold: match[2] }) // **bold**
    else if (match[3]) tokens.push({ italic: match[4] }) // _italic_
    else if (match[5]) tokens.push({ italic: match[6] }) // `italic`
    else if (match[7]) tokens.push({ link: { text: match[8], href: match[9] } }) // [text](url)

    lastIndex = pattern.lastIndex
  }

  if (lastIndex < str.length) {
    tokens.push(str.slice(lastIndex))
  }

  return tokens
})
</script>