<template>
  <div id="PokeCard" class="background">
    <div class="container">
      <header class="text-center">
        <img src="./assets/Pokemon.svg" width="400px" />
        <div>
          <img src="./assets/Whosthatpoke.png" width="100%" />
        </div>
        <h5>
          Pokemones descubiertos: <span>{{ counter }}/20</span>
        </h5>
      </header>
      <div class="row g-3">
        <div
          v-for="(pokemon, index) in shuffledPokemones"
          :key="index"
          class="col-3">
          <PokeCard :pokemon="pokemon" @respuestaCorrecta="incrementar" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import PokeCard from "./components/PokeCard.vue";

export default {
  name: "App",
  components: {
    PokeCard,
  },
  data() {
    return {
      pokemones: [],
      counter: 0,
    };
  },
  computed: {
    shuffledPokemones() {
      return this.shuffle(this.pokemones);
    },
  },
  methods: {
    async getPoke() {
      try {
        let offset = Math.floor(Math.random() * 1000); // Generate a random offset
        let response = await axios.get(
          `https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=20`
        );
        console.log(response);
        this.pokemones = response.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
    /* methods: {
     async getPoke() {
      try {
        let response = await axios.get("https://pokeapi.co/api/v2/pokemon");
         console.log(response);
         this.pokemones = response.data.results;
       } catch (error) {
         console.log(error);
      }
    },*/
    incrementar() {
      this.counter++;
    },
  },
  mounted() {
    this.getPoke();
  },
};
</script>
<style scoped>
/* * {
  background-color: aqua;
} */
.background {
  background-position: center;
  background-attachment: fixed;
  background-image: url("../src/assets/background3.jpg");
  background-size: cover;
}
h5 {
  font-family: "Jaro", sans-serif;
  font-optical-sizing: auto;
  font-weight: 400;
  font-size: 40px;
  color: rgba(255, 102, 0, 0.774);
  background-color: rgba(180, 249, 255, 0.492);
}
</style>
