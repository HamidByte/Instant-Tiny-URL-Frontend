<template>
  <div>
    <h1>Create Short URLs</h1>
    <p class="description">URL Shortener is a free tool to shorten URLs and generate short links URL shortener allows to create a shortened link making it easy to share</p>

    <input v-model="longUrl" type="text" placeholder="Enter a link to shorten it" />
    <button @click="shortenUrl">Shorten URL</button>

    <div v-if="shortId">
      Shortened URL:
      <RouterLink :to="shortId" target="_blank">{{ shortUrl }}</RouterLink>
    </div>

    <!-- Test -->
    <div v-else>
      <p>Example:</p>
      Shortened URL:
      <a href="http://127.0.0.1:5173/zpkgJpNLd" target="_blank">http://127.0.0.1:5173/zpkgJpNLd</a>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import axios from 'axios'

export default {
  setup() {
    // Define reactive variables using ref
    const longUrl = ref('')
    const shortId = ref('')
    const shortUrl = ref('')

    // Define the shortenUrl function
    const shortenUrl = async () => {
      try {
        const response = await axios.post(`${import.meta.env.VITE_API_URL}/shortener`, {
          longUrl: longUrl.value,
        }, {
          headers: {
            'Content-Type': 'application/json',
          },
        })
        const data = response.data
        shortId.value = data.shortId
        shortUrl.value = `${import.meta.env.VITE_BASE_URL}/${data.shortId}`
      } catch (error) {
        console.error('Error shortening URL:', error)
      }
    }

    // Return the variables and functions to be used in the template
    return {
      RouterLink,
      longUrl,
      shortId,
      shortUrl,
      shortenUrl,
    }
  },
}
</script>

<style>
</style>
