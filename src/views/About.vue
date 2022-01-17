<template>
  <router-link to="/">Go Back</router-link>
  <div class="poke_container">
    <div v-if="pokemon">
      <div className="pokemon">
        <div className="img_container">
          <img :src="pokemon.sprites.front_shiny" />
        </div>
        <div className="info">
          <span className="number">{{ $route.params.pokemon }}</span>
          <h3 className="name">{{ pokemon.name }}</h3>
          <small className="type"
            >Type:
            <div v-for="(type, idx) in pokemon.types" :key="idx">
              <span>{{ type.type.name }}</span>
            </div>
          </small>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { useRoute } from "vue-router";
import { reactive, toRefs } from "vue";

export default {
  setup() {
    const route = useRoute();

    const state = reactive({
      pokemon: null,
    });

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.pokemon}/`)
      .then((response) => response.json())
      .then((data) => {
        state.pokemon = data;
      });

    return {
      ...toRefs(state),
    };
  },
};
</script>
<style scoped>
a {
  background-color: #0078d0;
  border-radius: 10px;
  color: hsl(0, 0%, 100%);
  cursor: pointer;
  display: inline-block;
  font-family: system-ui, -apple-system, system-ui, "Segoe UI", Roboto, Ubuntu,
    "Helvetica Neue", sans-serif;
  font-size: 18px;
  font-weight: 600;
  padding: 16px 21px;
  position: relative;
  text-align: center;
  text-decoration: none;
  transition: all 0.3s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

a:before {
  background-color: initial;
  background-image: linear-gradient(#fff 0, rgba(255, 255, 255, 0) 100%);
  border-radius: 125px;
  content: "";
  height: 50%;
  left: 4%;
  opacity: 0.5;
  position: absolute;
  top: 0;
  transition: all 0.3s;
  width: 92%;
}

a:hover {
  box-shadow: rgba(255, 255, 255, 0.2) 0 3px 15px inset,
    rgba(0, 0, 0, 0.1) 0 3px 5px, rgba(0, 0, 0, 0.1) 0 10px 13px;
  transform: scale(1.05);
}

.poke_container {
  display: flex;
  flex-wrap: wrap;
  align-items: space-between;
  justify-content: center;
  margin: 0 auto;
  max-width: 1200px;
}

.pokemon {
  background-color: #eee;
  border-radius: 20px;
  box-shadow: 0 3px 15px rgba(100, 100, 100, 0.5);
  margin: 10px;
  padding: 20px;
  text-align: center;
  transition: all 0.3s ease-in-out;
}

.pokemon:hover {
  box-shadow: 0 3px 15px rgba(100, 100, 100, 0.8);
  transform: scale(1.05);
}

.pokemon .img_container {
  background-color: rgba(255, 255, 255, 0.6);
  border-radius: 50%;
  width: 120px;
  height: 120px;
  text-align: center;
}

.pokemon .img_container img {
  margin-top: 20px;
  max-width: 90%;
}

.pokemon .info {
  margin-top: 20px;
}

.pokemon .number {
  background-color: rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  font-size: 0.8em;
  padding: 5px 10px;
}

.pokemon .name {
  margin: 15px 0 7px;
  letter-spacing: 1px;
}

@media (min-width: 768px) {
  a {
    padding: 16px 48px;
  }
}
</style>
