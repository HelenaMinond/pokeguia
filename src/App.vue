<template>
  <div class="app">
    <!-- Encabezado -->
    <div class="encabezadoPokeguia">
      <img class="encabezadoPokeguia__img" src="./assets/titulo-pokemon.webp" alt="Título Pokemon">
      <h1>PokeGuía</h1>
    </div>
    <!-- Consulta -->
    <div class="consultaPokeguia">
      <label>Nombre:</label>
      <input type="text" v-model="pokemon">
      <button @click="obtenerPokemon()" >Buscar</button>
    </div>
    <!-- Resultado -->
    <div class="resultadoPokeguia">
      <img :src="imagenPokemon" alt="Imagen Pokemon">
      <h2>Movimientos</h2>
      <ul>
        <li v-for="(movimiento, indice) in movimientosPokemon" :key="indice">
          {{movimiento.move.name}}
        </li>
      </ul>
      <h2>Habilidades</h2>
      <ul>
        <li v-for="(habilidad, indice) in habilidadesPokemon" :key="indice">
          {{habilidad.ability.name}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      dataApi: "",
      pokemon: "pikachu",
    }
  },
  methods: {
    async obtenerPokemon() {
      const url = "https://pokeapi.co/api/v2/pokemon/";
      try {
        const request = await axios(url + this.pokemon);
        if(!request) return;
        this.dataApi = request.data;
        //console.log(this.dataApi);
      } catch (error) {
        console.log(error);
      }
    }
  },
  created () {
    this.obtenerPokemon();
  },
  computed: {
    imagenPokemon() {
      return this.dataApi && this.dataApi.sprites && this.dataApi.sprites.front_default; 
    },
    movimientosPokemon(){
      return this.dataApi.moves;
    },
    habilidadesPokemon(){
      return this.dataApi.abilities;
    }
  },
};
</script>

<style lang="scss">
.app{
  text-align: center;
  font-family: 'Arimo', sans-serif;
  color: rgb(66, 66, 66);
}

.encabezadoPokeguia {
  &__img {
    width: 30%;
    height: 30%;
  }
}

input:focus, input[type]:focus {
    border: 3px solid lightblue;
    border-radius: 5px;
    outline: 0 none;
}

li {
  list-style: none;
}
</style>
