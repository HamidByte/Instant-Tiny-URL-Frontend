<template>
  <div>
    <h1>Short URL Stats</h1>
    <input v-model="shortId" type="text" placeholder="Enter a shortId or short URL" />
    <button @click="getStats">Stats</button>

    <div v-if="stats">
      <p>Long URL: {{ stats.longUrl }}</p>
      <p>Short ID: {{ stats.shortId }}</p>
      <p>Short URL: {{ shortUrl }}</p>
      <p>Created At: {{ stats.createdAt }}</p>
      <p>Updated At: {{ stats.updatedAt }}</p>
      <p>Visit Count: {{ stats.visitCount }}</p>
    </div>

    <!-- Test -->
    <div v-else>
      <p>Example:</p>
      <p>Long URL: https://www.example.com/</p>
      <p>Short ID: zpkgJpNLd</p>
      <p>Short URL: http://127.0.0.1:5173/zpkgJpNLd</p>
      <p>Created At: 2024-01-05T12:25:28.587Z</p>
      <p>Updated At: 2024-01-05T12:25:28.587Z</p>
      <p>Visit Count: 5</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const shortId = ref('')
const stats = ref(null)
let shortUrl = ''

const getStats = async () => {
  try {
    const response = await axios.get(`${import.meta.env.VITE_API_URL}/stats/${shortId.value}`)
    stats.value = response.data
    shortUrl = `${import.meta.env.VITE_BASE_URL}/${stats.value.shortId}`
  } catch (error) {
    console.error("Error getting stats:", error)
  }
}
</script>

<style scoped>

</style>
