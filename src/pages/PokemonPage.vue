<template>
  <h1 v-if="!pokemon">Wait a sec please</h1>
  <div v-if="pokemon">
    <h1>¿Who is this Pokemon?</h1>
    <pokemon-picture v-bind:pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <pokemon-options v-bind:pokemons="pokemonArr" @selection="checkAnswer(1, $event)" />

    <div class="container-btn">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">Nuevo juego</button>
    </div>
  </div>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue';
import PokemonPicture from '@/components/PokemonPicture.vue';

import getPokemonOptions from '@/helpers/getPokemonOptions';

console.log(getPokemonOptions());

export default {
  components: {
    PokemonOptions,
    PokemonPicture,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnser: false,
      message: '',
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rndInt = Math.floor(Math.random() * 4);

      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(numero, pokemonId) {
      this.showPokemon = true;
      this.showAnser = true;

      if (pokemonId === this.pokemon.id) {
        this.message = `Good!, ${this.pokemon.name}`;
      } else {
        this.message = `Opps, sorry, ${this.pokemon.name}`;
      }
    },
    newGame() {
      this.showPokemon = false
      this.showAnser = false
      this.pokemonArr = []
      this.mixPokemonArray()
      this.pokemon = null
      this.message = ''
    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>

<style scoped>

</style>
