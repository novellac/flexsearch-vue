<template>
  <header>
    <h1>Flex Search example</h1>
  </header>

  <main>
    <h2>Search for dog breeds</h2>
    <p>This form will search through a static array of data using the flexsearch library. It uses only the default search config, so it only searches by whole word.</p>
    
    <form id="search" role="search" @submit.prevent="doSearch">
      <label for="search-input">Search all the breeds</label>
      <input type="search" id="search-input" name="search" spellcheck="false" v-model="query" />

      <button @click="doSearch()">Search</button>
    </form>

    <h2>Search results</h2>
    <p>{{ results.length }} showing</p>
    <ul>
      <li v-for="item in filteredData" :key="item.id">
        {{ item }}
      </li>
    </ul>

    <h2>Code output (just for reference)</h2>

    <h3>Your search query</h3>
    <code>{{ query }}</code>

    <h3>Your search results from the flexsearch index</h3>
    <code>{{ results }}</code>

    <h3>Your data, now filtered by flexsearch</h3>
    <code>{{ filteredData }}</code>

    <h3>The "index" that flexsearch built</h3>
    <code>{{ index }}</code>
  </main>
</template>

<script setup>
import Index from 'flexsearch'
import { computed, onMounted, ref } from 'vue'

const index = ref(new Index({}))
const query = ref('')
const results = ref([])

const rawData = [
  { id: 1, title: 'Affenpinscher' },
  { id: 2, title: 'Akita' },
  { id: 3, title: 'Braque Français' },
  { id: 4, title: 'Clumber Spaniel' },
  { id: 5, title: 'Dingo' },
  { id: 6, title: 'Jack Russell Terrier' },
  { id: 7, title: 'King Charles Spaniel' },
  { id: 8, title: 'Lhasa Apso' },
  { id: 8, title: 'Lobito Herreño' },
  { id: 10, title: 'Patagonian Sheepdog' },
  { id: 11, title: 'Whippet' },
  { id: 12, title: 'Whippet Two' }
]

const buildIndex = () => {
  Object.values(rawData).forEach((item) => {
    index.value.add(item.id, item.title)
  })
}

const doSearch = () => {
  results.value = index.value.search(query.value)
}

const filteredData = computed(() => {
  return rawData.filter((item) => results.value.includes(item.id))
})

onMounted(() => {
  buildIndex()
})
</script>
