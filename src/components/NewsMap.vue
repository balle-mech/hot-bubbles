<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import NewsItem from './NewsCard.vue'

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
  <div class="news-container">
    <div v-if="news">
      <div class="news-column large">
        <NewsItem
          v-for="(result, index) in news.results.slice(0, 1)"
          :key="result.title"
          :result="result"
          size="large"
        />
      </div>
      <div class="news-column medium">
        <NewsItem
          v-for="(result, index) in news.results.slice(1, 3)"
          :key="result.title"
          :result="result"
          size="medium"
        />
      </div>
      <div class="news-column small">
        <NewsItem
          v-for="(result, index) in news.results.slice(3, 6)"
          :key="result.title"
          :result="result"
          size="small"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.news-container {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}

.news-column {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  flex: 1;
}

.news-column.large {
  flex: 1;
}

.news-column.medium {
  flex: 1;
}

.news-column.small {
  flex: 1;
}
</style>
