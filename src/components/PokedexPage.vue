<template>
  <div class="main">
    <h1>Pokedex</h1>
    <div v-for="pokemon in pokemonList.results" v-bind:key="pokemon">
      <div class="pname" v-on:click="getPokemon(pokemon.url)">
        {{pokemon.name}}
      </div>
    </div>
    <PokemonModal v-if="this.showModal" :pokemon="this.pokemon" @closeModal="updateModalState"/>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonModal from './PokemonModal'

export default {
  name: 'PokedexPage',
  components: {
    PokemonModal
  },
  created: function() {
      document.title = 'Pokedex encyclopedia'
  },
  mounted() {
    this.getPokemonList(0, 10)
  },
  data() {
    return {
      pokemonList: [],
      pokemon: {},
      showModal: false,
    }
  },
  methods: {
    async getPokemonList(start, number) {
      try {
        await axios.get(
          `https://pokeapi.co/api/v2/pokemon?limit=${number}&offset=${start}`
        ).then(res => {
          this.pokemonList = res.data
        })
      } catch (e) {
        console.error(e)
      }
    },
    async getPokemon(url) {
      try {
        await axios.get(url).then(res => {
          this.pokemon = res.data
          console.log(this.pokemon)
        })
      } catch (e) {
        console.error(e)
      }
    },
    updateModalState(val) {
      this.showModal = val
    }
  },
  watch: {
    pokemon() {
      this.showModal = !this.showModal
    }
  }
}
</script>

<style lang="scss" scoped>
.pname {
  cursor: pointer;
}
</style>
