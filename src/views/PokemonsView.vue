<script setup>
import { ref } from 'vue'

const pokemons = ref([])
const offset = ref(1)

const interval = {
  offset,
  limit: 10
}

const P = new Pokedex.Pokedex(interval)

P.getPokemonsList().then((response) => {
  pokemons.value = response.results
  return pokemons
})
</script>

<template>
  <div class="about">
    <h1>Pokemons</h1>
    <div v-if="pokemons">
      <div v-for="(pokemon, index) in pokemons" :key="index">
        {{ pokemon.name }}
      </div>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    flex-direction: column;
  }
}
</style>
