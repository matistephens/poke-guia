<template>
  <main>
    <section class="banner">
      <img src="pokemon-banner.jpeg" />
    </section>

    <section class="search">
      <h4>PokeGuía</h4>
      <form class="input-search" @submit.prevent="buscarPokemon(pokemonIngresado)">
        <p>Nombre:</p>
        <input v-model="pokemonIngresado" type="text" placeholder="Ingresa el nombre del pokemon" required/>
        <button type="submit">Buscar</button>
      </form>
    </section>

    <section>
      <img :src="pokemonImage" alt="pokemon">
    </section>

    <section class="movements">
      <h4>Movimientos</h4>
      <ul v-for="(move, $index) in pokemonMoves" :key="$index">
        {{ move }}
      </ul>
    </section>

    <section class="habilities">
      <h4>Habilidades</h4>
      <ul v-for="(ability, $index) in pokemonAbilities" :key="$index">
        {{ ability }}
      </ul>
    </section>
  </main>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    pokemon: null,
    pokemonIngresado: "",
    pokemonMoves: [],
    pokemonAbilities: [],
    pokemonImage: ""
  }),
  mounted() {
    this.buscarPokemon("pikachu")
  },
  computed: {
    savePokemonMoves() {
      this.pokemonMoves = this.pokemon.moves.map((move) => move.move.name)
    },
    savePokemonAbilities() {
      this.pokemonAbilities = this.pokemon.abilities.map((ability) => ability.ability.name)
    },
    savePokemonImage() {
      this.pokemonImage = pokemon.sprites.front_default
    }

  },
  methods: {
    async buscarPokemon(pokemonIngresado) {
      const pokemon = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${pokemonIngresado}`
      ).then((response) => response.json())
      this.pokemon = pokemon

      this.pokemonMoves = pokemon.moves.map((move) => move.move.name)
      this.pokemonAbilities = pokemon.abilities.map((ability) => ability.ability.name)
      this.pokemonImage = pokemon.sprites.front_default
      console.log(pokemon.sprites.front_default)

    },
  },
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: black;
  color: white;
}

main {
  max-width: 400px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-left: auto;
  margin-right: auto;
}

.banner {
  display: flex;
  align-items: center;
  justify-content: center;
}
.banner img {
  width: 50%;
}

.search {
  text-align: center;
}

.search .input-search {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
}

.search .input-search p {
  margin: 0;
}

img {
  width: 10rem;
}
</style>
