<template>
  <div class="text-center py-3">
    <div class="card">
      <img :src="infoPokemon.sprites?.other['official-artwork'].front_default" class="card-img-top"
        :class="{ borroso: esconderPokemon }" />
      <div class="card-body">
        <h5 class="text-center fw-bold" v-show="!esconderPokemon">{{ pokemon.name }}</h5>
        <form v-show="esconderPokemon">
          <input class="form-control" type="text" placeholder="Escribe aquí el nombre" v-model="nombre">
          <div class="d-grid">
            <button class="btn btn-info" type="submit" @click.prevent="comprobarNombre">Descubrir</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "PokeCard",
  props: {
    pokemon: Object,
  },
  data() {
    return {
      infoPokemon: {},
      esconderPokemon: true,
      nombre: '',
    }
  },
  created() {
    this.getinfoPokemon()
  },
  methods: {
    async getinfoPokemon() {
      try {
        let { data } = await axios.get(this.pokemon.url)
        this.infoPokemon = data
      } catch (error) {
        console.log(error)
      }
    },
    comprobarNombre() {
      if (this.nombre.toLowerCase() === this.infoPokemon.name.toLowerCase()) {
        this.esconderPokemon = false
        this.$emit("respuestaCorrecta")
      } else {
        this.esconderPokemon = true
        this.$swal.fire({
          title: 'Ups!',
          text: '`Intenta nuevamente`',
          icon: 'error',
          confirmButtonText: 'Continuar'
        })
      }
    }
  }
};
</script>

<style scoped>
button {
  font-family: "Orbitron", sans-serif;
  font-optical-sizing: auto;
  font-weight: 400;
}

img {
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
.card {
  background-color: rgba(180, 249, 255, 0.492);
  border-radius: 10%;
}

.card-img-top.borroso {
  filter: blur(5px) grayscale(100%);
}
h5 {
  font-family: "Press Start 2P", system-ui;
  font-weight: 400;
}
</style>