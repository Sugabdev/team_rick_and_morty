<script setup lang="ts">
import { onMounted } from 'vue'
import { ref } from 'vue'
import type { Character } from '../types/Character.ts'

const characters = ref<Character[]>([])
const page = ref<number>(1)
const nextPageStatus = ref<boolean>()
const prevPageStatus = ref<boolean>()
const inputQuery = ref<string>('')

const loadCharacters = async () => {
  const res = await fetch(`https://rickandmortyapi.com/api/character/?page=${page.value}`)
  const data = await res.json()

  prevPageStatus.value = data.info.prev
  nextPageStatus.value = data.info.next

  characters.value = data.results
}

const searchCharacters = () => {
  characters.value.filter((character) => {
    return character.name.toLowerCase().includes(inputQuery.value.toLowerCase())
  })
  loadCharacters()
}

const nextPage = () => {
  if (!nextPageStatus.value) return
  page.value++
  loadCharacters()
}

const prevPage = () => {
  if (!prevPageStatus.value) return
  page.value--
  loadCharacters()
}

onMounted(() => {
  loadCharacters()
})
</script>

<template>
  <main class="min-h-screen flex flex-col justify-center items-center gap-6 p-6 md:gap-12 md:p-12">
    <header id="header">
      <h1 class="text-center text-[2rem]">Rick and Morty Api</h1>
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

    <section id="filter" class="flex flex-row overflow-hidden rounded-lg border border-gray-400">
      <input v-model="inputQuery" type="text" placeholder="Type any character . . ." class="p-2" />
      <button
        @click="searchCharacters"
        class="bg-cyan-600 px-2 hover:bg-cyan-700 hover:duration-100"
      >
        search
      </button>
    </section>

    <section id="characters-container" class="flex flex-wrap justify-center gap-12">
      <article
        class="flex flex-col rounded-lg overflow-hidden w-[250px] hover:transform hover:scale-110 transition-transform duration-300"
        v-for="character in characters"
        :key="character.id"
      >
        <img :src="character.image" :alt="character.name" />
        <div
          class="flex flex-col items-center justify-center w-full h-[40%] text-center p-4 bg-gray-100 text-gray-800 text-lg font-bold"
        >
          <h2>{{ character.name }}</h2>
        </div>
      </article>
    </section>
  </main>
</template>
