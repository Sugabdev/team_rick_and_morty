<script setup lang="ts">
import { onMounted } from 'vue'
import { ref } from 'vue'
import type { Character } from '../types/Character.ts'

const characters = ref<Character[]>([])

const fetchData = async () => {
  const res = await fetch('https://rickandmortyapi.com/api/character')
  const data = await res.json()
  characters.value = data.results
  console.log(data.results)
}

onMounted(() => {
  fetchData()
})
</script>

<template>
  <main class="min-h-screen flex flex-col justify-center items-center gap-2 p-6">
    <header id="header">
      <h1 class="text-center text-[3rem]">Rick and Morty Api</h1>
    </header>

    <section id="pagination">
      <button></button>
      <span> {{}}</span>
      <button></button>
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
