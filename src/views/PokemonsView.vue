<script setup>
import { ref, watch } from 'vue'

const pokemons = ref([])
const offset = ref(1)

const getPokemons = (interval) => {
  const P = new Pokedex.Pokedex(interval)

  P.getPokemonsList().then((response) => {
    pokemons.value = response.results
    return pokemons
  })
}

getPokemons({
  offset,
  limit: 10
})

const avanca = () => offset.value++
const volta = () => offset.value--

watch(offset, (newOffset) => {
  getPokemons({
    offset: newOffset,
    limit: 10
  })
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
    <div class="navigator">
      <b v-if="offset > 1" @click="volta">Voltar</b>
      <b v-if="pokemons.length" @click="avanca">Avançar</b>
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
.navigator {
  margin-top: 20px;
  display: flex;
  gap: 10px;
}

b {
  cursor: pointer;
}
</style>
