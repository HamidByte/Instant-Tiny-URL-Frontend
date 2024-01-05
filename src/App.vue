<template>
  <header v-if="!is404Route">
    <nav>
      <div class="left-nav">
        <RouterLink to="/shortener">Home</RouterLink>
        <RouterLink to="/stats/:shortUrl">Stats</RouterLink>
      </div>
      <div class="right-nav">
        <RouterLink to="/about">About</RouterLink>
      </div>
    </nav>
  </header>

  <RouterView />
</template>

<script setup>
import { ref, watchEffect } from 'vue'
import { RouterLink, RouterView, useRouter } from 'vue-router'

const is404Route = ref(false)
const router = useRouter()

watchEffect(() => {
  // Check if the current route is the "PageNotFound" route
  is404Route.value = router.currentRoute.value.name === 'PageNotFound'
})
</script>

<style scoped>

</style>
