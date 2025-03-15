<script setup lang="ts">
import { onMounted } from 'vue'
import { ref } from 'vue'
import type { Character } from '../types/Character.ts'

const characters = ref<Character[]>([])
const page = ref<number>(1)

const loadCharacters = async () => {
  const res = await fetch(`https://rickandmortyapi.com/api/character/?page=${page.value}`)
  const data = await res.json()
  characters.value = data.results
  console.log(data.results)
}

const nextPage = () => {
  page.value++
  loadCharacters()
}

const prevPage = () => {
  if (page.value === 1) return
  page.value--
  loadCharacters()
}

onMounted(() => {
  loadCharacters()
})
</script>

<template>
  <main class="min-h-screen flex flex-col justify-center items-center gap-2 p-6">
    <header id="header">
      <h1 class="text-center text-[3rem]">Rick and Morty Api</h1>
    </header>

    <section id="pagination">
      <button
        @click="prevPage"
        class="text-white bg-blue-700 hover:bg-blue-800 focus:outline-none focus:ring-4 focus:ring-blue-300 font-medium rounded-full text-sm px-5 py-2.5 text-center me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
      >
        {{ '<-' }}
      </button>
      <span
        class="py-2.5 px-5 me-2 mb-2 text-sm font-medium text-gray-900 focus:outline-none bg-white rounded-full border border-gray-200 hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-gray-100 dark:focus:ring-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:border-gray-600 dark:hover:text-white dark:hover:bg-gray-700"
      >
        {{ page }}
      </span>
      <button
        @click="nextPage"
        class="text-white bg-blue-700 hover:bg-blue-800 focus:outline-none focus:ring-4 focus:ring-blue-300 font-medium rounded-full text-sm px-5 py-2.5 text-center me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
      >
        {{ '->' }}
      </button>
    </section>

    <section id="characters-container" class="flex flex-wrap justify-center gap-8">
      <article
        class="rounded-lg overflow-hidden"
        v-for="character in characters"
        :key="character.id"
      >
        <img :src="character.image" :alt="character.name" />
        <div class="w-full text-center p-4 bg-gray-100 text-gray-800 text-lg font-bold">
          <h2>{{ character.name }}</h2>
        </div>
      </article>
    </section>
  </main>
</template>
