<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import ImageSearchPage from './components/ImageSearchPage.vue'
</script>

<template>
  <header>
    <div class="wrapper"></div>
  </header>

  <main>
    <ImageSearchPage> </ImageSearchPage>
  </main>

  <footer></footer>
  <RouterView />
</template>

<style scoped></style>
