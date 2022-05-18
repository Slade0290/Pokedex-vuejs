<template>
  <div class="modal">
    <div class="modal-content">  
      <span class="close" v-on:click="this.closeModal()">&times;</span>
      <div class="sub-content">
        <h1 class="pname">{{currentpokemon.name}}</h1>
        <img :src="currentpokemon.sprites.front_default">
        <p>Height: {{(currentpokemon.height)*10}} cm</p>
        <p>Weight: {{(currentpokemon.weight)/10}} kg</p>
        <p>Type:
          <span v-for="ptype in currentpokemon.types" v-bind:key="ptype">
            <span class="type">{{ptype.type.name}}</span> 
          </span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokemonModal',
  props: {
    pokemon: {}
  },
  data() {
    return {
      showModal: false,
      currentpokemon: this.pokemon
    }
  },
  mounted() {
    this.initPageInfo()
  },
  methods: {
    initPageInfo() {
      document.title = `Pokedex encyclopedia - ${this.pokemon.name}`
    },
    closeModal() {
      this.$emit('closeModal', this.showModal)
    }
  }
}
</script>

<style lang="scss" scoped>
  .modal {
    position: fixed; 
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0,0,0);
    background-color: rgba(0,0,0,0.4);
    .modal-content {
      background-color: #fefefe;
      border-radius: 1rem;
      margin: 15% auto; 
      padding: 20px;
      border: 1px solid #888;
      box-shadow: .2rem .2rem .5rem .1rem rgba(25,25,25,0.8);
      width: 80%;
      .close {
        color: #aaa;
        float: right;
        font-size: 28px;
        font-weight: bold;
        transition: .5s;
        &:hover,
        &:focus {
          color: black;
          text-decoration: none;
          cursor: pointer;
        }
      }
      .sub-content {
        margin: 3rem;
        .pname {
          text-transform: capitalize;
        }
        .type {
          padding-right: 5px;
        }
      }
    }
  }
</style>
