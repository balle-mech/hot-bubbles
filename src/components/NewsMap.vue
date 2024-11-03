<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const news = ref(null)
const newsApiKey = import.meta.env.VITE_NEWS_API_KEY

const fetchNews = async () => {
  try {
    const res = await axios.get(
      'https://newsapi.org/v2/top-headlines?country=us&apiKey=' + newsApiKey,
    )
    news.value = res.data
  } catch (error) {
    console.error('Error fetching data:', error)
  }
}

onMounted(() => {
  fetchNews()
  console.log(import.meta.env.MODE)
})
</script>

<template>
  <div>
    <h1>NewsMap</h1>
    <div v-if="news">
      <ul>
        <li v-for="article in news.articles" :key="article.title">
          <h2>{{ article.title }}</h2>
        </li>
      </ul>
    </div>
  </div>
</template>
