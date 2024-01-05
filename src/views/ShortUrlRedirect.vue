<template>
  <div class="content">
    <p v-if="loading">Redirecting...</p>
    <!-- Show 404 error component if shortId is not found -->
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import axios from 'axios'

export default {
  setup() {
    const route = useRoute()
    const router = useRouter()
    const loading = ref(true)

    onMounted(async () => {
      // Access the shortId parameter from the route
      const shortId = route.params.shortId

      // Construct the URL for checking existence
      const checkUrl = `${import.meta.env.VITE_API_URL}/check/${shortId}`;

      
      try {
        // Check if the URL exists
        await axios.head(checkUrl)

        // If the request is successful, construct the short URL for redirection
        const shortUrl = `${import.meta.env.VITE_API_URL}/${shortId}`;

        // Redirect to the short URL
        window.location.href = shortUrl;
      } catch (error) {
        // If an error occurs (404 or any other status), show the 404 error component
        loading.value = false
        // Redirect to the 404 error route
        router.replace({ name: 'PageNotFound' })
      }
    })

    return {
      loading,
    }
  },
}
</script>

<!-- <script>
export default {
  beforeRouteEnter(to, from, next) {
    const shortId = to.params.shortId;
    const shortUrl = `${import.meta.env.VITE_API_URL}/${shortId}`;

    // Redirect to the short URL
    window.location.href = shortUrl;

    // Do not call next() to prevent the component from being created
    // This component will not be created as the user will be redirected
  },
};
</script> -->