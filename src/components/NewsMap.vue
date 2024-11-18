<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const news = ref(null)
const newsApiKey = import.meta.env.VITE_NEWS_API_KEY

const fetchNews = async () => {
  try {
    const res = await axios.get(
      'https://newsdata.io/api/1/latest?apikey=' + newsApiKey + '&language=jp',
    )
    news.value = res.data
  } catch (error) {
    console.error('Error fetching data:', error)
  }
}

onMounted(() => {
  fetchNews()
})
</script>

<template>
  <div>
    <h1>NewsMap</h1>
    <div v-if="news">
      <ul>
        <li v-for="result in news.results" :key="result.title">
          <a v-bind:href="result.source_url">{{ result.title }}</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}

li {
  margin-top: 1rem;
}
</style>
