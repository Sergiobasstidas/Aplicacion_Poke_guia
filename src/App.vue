<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top px-5">
      <a class="navbar-brand" href="#">Guía del Pokémon</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarNavAltMarkup"
        aria-controls="navbarNavAltMarkup"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav ml-auto">
          <a class="nav-link active" href="#"
            >Home <span class="sr-only">(current)</span></a
          >
          <a class="nav-link" href="#">Features</a>
          <a class="nav-link" href="#">Contact</a>
        </div>
      </div>
    </nav>

    <img class="logo" src="./assets/pokemon-logo.jpg" />
    <h1>PokeGuía</h1>
    <div>
      <label>Nombre del Pokémon:</label>
      <input
        class="ml-1 mr-2 py-1"
        type="text"
        placeholder="Ingrese el nombre"
        v-model="nombrePokemon"
      />
      <button class="btn btn-secondary" type="button" @click="buscarPokemon">
        Buscar
      </button>
    </div>
    <div>
      <img :src="frontDefault" />
    </div>
    <div>
      <h2>Movimientos</h2>
      <p v-for="(movimiento, $index) in moves" :key="$index">
        {{ movimiento.move.name }}
      </p>
    </div>
    <div>
      <h2>Habilidades</h2>
      <p v-for="(habilidad, $index) in abilities" :key="$index">
        {{ habilidad.ability.name }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    nombrePokemon: "",
    pokemon: null,
  }),
  methods: {
    resetPokemon() {
      this.pokemon = null;
    },
    async buscarPokemon() {
      if (this.nombrePokemon !== "") {
        this.getPokemon(this.nombrePokemon);
      }
    },
    async getPokemon(nombrePokemon) {
      try {
        let response = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${nombrePokemon}`
        );
        let data = await response.json();
        this.pokemon = data;
      } catch (e) {
        console.error(e);
      }
    },
  },
  computed: {
    frontDefault() {
      return (
        (this.pokemon &&
          this.pokemon.sprites &&
          this.pokemon.sprites.front_default) ||
        ""
      );
    },
    moves() {
      return (this.pokemon && this.pokemon.moves) || [];
    },
    abilities() {
      return (this.pokemon && this.pokemon.abilities) || [];
    },
  },
  created() {
    this.nombrePokemon = "pikachu";
    this.buscarPokemon();
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo {
  width: 90vh;
}
.lista {
  list-style: none;
  width: 50vh;
  margin: 0 auto;
}
img {
  width: 10%;
}
</style>
© 2021 GitHub, Inc.
