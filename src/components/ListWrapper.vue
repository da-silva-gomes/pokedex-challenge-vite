<script lang='ts'>
import axios from 'axios'
import TypesList from './TypesList.vue'
import PokemonList from './PokemonList.vue'

const TYPE_API_URL = `https://pokeapi.co/api/v2/type?limit=18`;
const POKEMONS_API_URL = `https://pokeapi.co/api/v2/type/`;

export default {
  name: 'ListWrapper',
  data() {
    return {
      typesList: [],
      pokemonsList: [],
    };
  },
  components: {
    TypesList,
    PokemonList,
  },
  computed: {
    pokemonListHasData() {
      return this.pokemonsList.length !== 0;
    },
  },
  async created() {
    try {
      const types = await axios.get(`${TYPE_API_URL}`)

      types.data.results.forEach((type) => {
        this.typesList = [...this.typesList, type.name]
      })
    } catch (error) {
      console.log('FETCH TYPES REQUEST - ERROR:', error)
    }
  },
  methods: {
    async searchType(type: string) {
      try {
        const pokemons = await axios.get(`${POKEMONS_API_URL}${type}`);
        this.pokemonsList = pokemons.data.pokemon;
      } catch (error) {
        console.log('FETCH TYPE POKEMONS REQUEST - ERROR:', error);
      }
    },
  },
}
</script>

<template>
  <div class='px-16 py-4'>
    <TypesList :types='typesList' @type-selected='searchType' />
    <PokemonList :pokemon-list='pokemonsList' />
  </div>
</template>
