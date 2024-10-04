<script setup>
import { ref, watch } from 'vue'

const pokemons = ref([])
const offset = ref(1)
const total = ref(0)
const limit = 10
const pages = ref(0)

const getPokemons = (interval) => {
  const P = new Pokedex.Pokedex(interval)

  P.getPokemonsList().then((response) => {
    total.value = response.count
    pages.value = Math.round(response.count / limit)
    pokemons.value = response.results
    return pokemons
  })
}

getPokemons({
  offset,
  limit
})

const skip = (e) => (offset.value = Number(e.target.dataset.offset))
watch(offset, (newOffset) => {
  getPokemons({
    offset: newOffset,
    limit
  })
})
</script>

<template>
  <div class="pokemons">
    <h1>Pokemons</h1>
    <div v-if="pokemons">
      <div v-for="(pokemon, index) in pokemons" :key="index">
        {{ pokemon.name }}
      </div>
    </div>
    <div class="navigator">
      <template v-for="page in pages" :key="page">
        <span
          :class="{ current: page == offset }"
          @click="skip"
          :data-offset="page"
          v-if="page == offset + 2"
          >{{ page }}</span
        >
        <span
          :class="{ current: page == offset }"
          @click="skip"
          :data-offset="page"
          v-if="page == offset + 1"
          >{{ page }}</span
        >
        <span
          :class="{ current: page == offset }"
          @click="skip"
          :data-offset="page"
          v-if="page == offset"
          >{{ page }}</span
        >
        <span
          :class="{ current: page == offset }"
          @click="skip"
          :data-offset="page"
          v-if="page == offset - 1"
          >{{ page }}</span
        >
        <span
          :class="{ current: page == offset }"
          @click="skip"
          :data-offset="page"
          v-if="page == offset - 2"
          >{{ page }}</span
        >
      </template>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .pokemons {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    flex-direction: column;
  }
}
.navigator {
  margin-top: 20px;
}

.current {
  background: #41b883;
}

b,
span {
  cursor: pointer;
}

span {
  padding: 0px 5px;
  border-radius: 5px;
}
</style>
