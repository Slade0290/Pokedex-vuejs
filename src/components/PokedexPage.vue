<template>
  <div>
    Hello
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'PokedexPage',
  created: function() {
      document.title = 'Pokedex encyclopedia'
  },
  mounted() {
    this.getPokemon(100)
  },
  data() {
    return {
      posts: [],
    }
  },
  methods: {
    async getPokemon(number) {
      try {
        await axios.get(
          `https://pokeapi.co/api/v2/pokemon?limit=${number}&offset=0`
        ).then(res => {
          this.posts = res.data
          console.log(this.posts)
        })
      } catch (error) {
        console.log(error);
      }
    },
  },
}
</script>

<style scoped>

</style>
