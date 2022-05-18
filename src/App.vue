<template>
  <PokedexPage />
</template>

<script>
import PokedexPage from './components/PokedexPage.vue'

export default {
  name: 'App',
  components: {
    PokedexPage
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
