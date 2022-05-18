<template>
  <div class="main">
    <h1 class="title">Pokedex</h1>
    <div class="list-section">
        <div class="pitem" v-for="pokemon in pokemonList.results" :key="pokemon">
        <div class="pname" v-on:click="getPokemon(pokemon.url)">
          {{pokemon.name}}
        </div>
      </div>
    </div>
    <PokemonModal v-if="this.showModal" :pokemon="this.pokemon" @closeModal="updateModalState"/>
    <div class="button-section">
      <div class="previous button" v-on:click="prev()" v-if="this.first > 0">Prev</div>
      <select v-model.number="unit">
        <option>20</option>
        <option>50</option>
        <option>100</option>
      </select>
      <div class="next button" v-on:click="next()">Next</div>
    </div>
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
    this.getPokemonList()
    this.number = this.unit
  },
  data() {
    return {
      pokemonList: [],
      pokemon: {},
      showModal: false,
      unit: 20,
      first: 0,
      number: 0,
    }
  },
  methods: {
    async getPokemonList() {
      try {
        await axios.get(
          `https://pokeapi.co/api/v2/pokemon?limit=${this.number}&offset=${this.first}`
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
    },
    prev() {
      this.first -= this.unit;
    },
    next() {
      this.first += this.unit;
    },
  },
  watch: {
    pokemon() {
      this.showModal = !this.showModal
    },
    first() {
      this.getPokemonList()
    },
    unit() {
      this.number = this.unit
      this.getPokemonList()
    }
  }
}
</script>

<style lang="scss" scoped>
@font-face {
  font-family: 'Pokemon';
  src: url('../assets/fonts/Pokemon\ Hollow.ttf')
}
$pyellow: rgb(255, 203, 5);
$pblue: rgb(60, 90, 166); 

.main {
  .title {
    font-family: 'Pokemon';
    font-size: 5rem;
    background: rgb(230,50,50);
    color: $pyellow;
    text-shadow: .1rem .1rem .1rem $pblue;
    border-left: black 1rem solid;
  }
  .list-section {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    .pitem {
      padding: 1rem;
      border: solid 1px lightblue;
      margin: .5rem;
      .pname {
        text-transform: capitalize;
        cursor: pointer;
        transition: .5s;
        &:hover {
          letter-spacing: .1rem;
        }
      }
    }
  }
}
.button-section {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  .button {
    cursor: pointer;
    background: green;
    border-radius: 5px;
    padding: 8px;
    color: white;
    font-weight: bold;
    transition: .5s;
    &:hover {
      background: rgba(0,150,50,0.8);
    }
    &:active {
      background: rgba(0,250,50,0.8);
    }
  }
}
</style>
