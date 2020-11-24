<template>
  <main>
    <ol>
      <li v-for="pokemon in pokemons">
        <img
          :src="pokemon.sprites.other['official-artwork'].front_default"
          :alt="pokemon.name"
          width="475"
          height="475"
          loading="lazy"
        />
        <h1>{{ pokemon.name }}</h1>
      </li>
    </ol>
  </main>
</template>

<script>
export default {
  async asyncData() {
    fetchPokemons('https://pokeapi.co/api/v2/pokemon');

    const pokemons = [];

    async function fetchPokemons(url) {
      const response = await fetch(url);
      const data = await response.json();

      for (const pokemon of data.results) {
        const pokemonData = await fetchPokemon(pokemon.url);
        pokemons.push(pokemonData);
      }
    }

    async function fetchPokemon(url) {
      const response = await fetch(url);
      const data = await response.json();
      return data;
    }


    return { pokemons };
  }
}
</script>

<style scoped>
main {
  padding-bottom: 80px;
}

ol {
  padding: 24px;
  list-style: none;

  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 12px;
}

ol li {
  position: relative;
  padding: 24px;
  border-radius: 4px;
  background-color: #fff;
  cursor: pointer;

  will-change: transform;
  transition: transform 150ms ease;
}

ol li:hover {
  z-index: 1;
  transform: scale(1.1);
  box-shadow: 0 4px 12px 0 rgba(0, 0, 0, 0.15);
}

ol li a {
  text-decoration: none;
  color: inherit;
}

ol li img {
  display: block;
  width: 100%;
  height: auto;
}

ol li h1 {
  margin-top: 24px;
  font-size: 24px;
  font-weight: 700;
  text-align: center;
}
</style>
