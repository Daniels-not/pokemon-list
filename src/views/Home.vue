<template>
  <div class="home">
    <div class="container">
      <div class="text_box_container">
        <input type="text" placeholder="pokemon name" v-model="pokemonName" />
      </div>
      <div class="pokemon_container">
        <div
          class="pokemon"
          v-for="(pokemon, idx) in filteredPokemons"
          :key="idx"
        >
          <router-link :to="`/about/${pokemonURL[pokemon.name]}`">{{
            pokemon.name
          }}</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { computed, reactive, toRefs } from "vue";

export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemon: [],
      pokemonURL: {},
      pokemonName: "",
      filteredPokemons: computed(() => filter()),
    });

    function filter() {
      if (!state.pokemonName) {
        return [];
      } else {
        return state.pokemon.filter((pokemon) =>
          pokemon.name.includes(state.pokemonName)
        );
      }
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        state.pokemon = data.results;
        state.pokemonURL = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      });

    return {
      ...toRefs(state),
    };
  },
};
</script>
<style scoped>
input {
  padding: 10px;
  border: 1px solid hsl(0, 0%, 80%);
  border-radius: 4px;
  font-size: 16px;
  margin-block: 1rem;
}

a {
  color: #000;
  text-decoration: none;
  letter-spacing: 0.2em;
}
</style>
