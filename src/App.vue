<template>
  <div class="app" id="app">
    <Container />
  </div>
</template>

<script setup lang="ts">
import Container from "./components/Container.vue";
console.log('----- App')
</script>

<style scoped>
.app {
  @apply h-screen w-screen flex justify-center items-center bg-lime-500 bg-opacity-50;
}
</style>
