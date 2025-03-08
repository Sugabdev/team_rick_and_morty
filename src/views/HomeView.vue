<script setup lang="ts">
import { onMounted } from 'vue'
import { ref } from 'vue'

const characters = ref([])

const fetchData = async () => {
  const res = await fetch('https://rickandmortyapi.com/api/character')
  const data = await res.json()
  characters.value = data.results
  console.log(characters)
}

onMounted(() => {
  fetchData()
})
</script>

<template>
  <main>
    <header>
      <h1 class="text-center">Rick and Morty Api</h1>
    </header>

    <section class="flex flex-wrap justify-center gap-8">
      <article v-for="character in characters" :key="character.id">
        <img class="rounded-lg" :src="character.image" :alt="character.name" />
      </article>
    </section>
  </main>
</template>
