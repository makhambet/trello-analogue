<template>
  <div id="app">
    <h1>Trello</h1>
    <app-card></app-card>
  </div>
</template>

<script>
import AppCard from './components/card.vue'

export default {
  name: 'App',
  components: {
    AppCard
  }
}
</script>

<style lang="scss">
  body {
    overflow: hidden;
    width: 100%;
    height: 100vh;
  }
  h1 {
    text-align: center;
  }
</style>
