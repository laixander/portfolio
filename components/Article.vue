<template>
    <article class="prose max-w-none">
        <FancyHeading v-if="title" :title="title" :highlight="highlight" :size="size" />
        <FancyDivider v-if="hasDivider" />
        <p v-for="(p, i) in parsedParagraphs" :key="i" class="font-light text-gray-500 dark:text-gray-400 text-pretty">
            <template v-for="(chunk, j) in p" :key="j">
                <template v-if="typeof chunk === 'string'">
                    {{ chunk }}
                </template>
                <template v-else-if="'bold' in chunk">
                    <strong class="font-semibold text-gray-700 dark:text-gray-300">
                        {{ chunk.bold }}
                    </strong>
                </template>
                <template v-else-if="'italic' in chunk">
                    <em class="italic text-gray-600 dark:text-gray-400">
                        {{ chunk.italic }}
                    </em>
                </template>
                <template v-else-if="'link' in chunk">
                    <a :href="chunk.link.href" class="text-primary-600 hover:underline dark:text-primary-400">
                        {{ chunk.link.text }}
                    </a>
                </template>
                <template v-else-if="'code' in chunk">
                    <code class="bg-gray-100 dark:bg-gray-800 px-1 rounded text-sm">
                        {{ chunk.code }}
                    </code>
                </template>
            </template>
        </p>
    </article>
</template>

<script setup lang="ts">
import { computed } from 'vue'

type Chunk =
    | string
    | { bold: string }
    | { italic: string }
    | { link: { text: string; href: string } }
    | { code: string }

const props = withDefaults(
    defineProps<{
        title?: string
        highlight?: string
        size?: string
        hasDivider?: boolean
        paragraphs?: string[]
    }>(),
    {
        hasDivider: false,
    }
)

function parseParagraph(paragraph: string): Chunk[] {
    const chunks: Chunk[] = []
    const regex =
        /(\*\*(.+?)\*\*|_(.+?)_|`(.+?)`|\[(.+?)\]\((https?:\/\/[^\s]+)\))/g

    let lastIndex = 0
    let match

    while ((match = regex.exec(paragraph)) !== null) {
        if (match.index > lastIndex) {
            chunks.push(paragraph.slice(lastIndex, match.index))
        }

        if (match[2]) {
            chunks.push({ bold: match[2] })
        } else if (match[3]) {
            chunks.push({ italic: match[3] })
        } else if (match[4]) {
            chunks.push({ code: match[4] })
        } else if (match[5] && match[6]) {
            chunks.push({ link: { text: match[5], href: match[6] } })
        }

        lastIndex = regex.lastIndex
    }

    if (lastIndex < paragraph.length) {
        chunks.push(paragraph.slice(lastIndex))
    }

    return chunks
}

const parsedParagraphs = computed(() =>
    props.paragraphs?.map(parseParagraph) ?? []
)
</script>