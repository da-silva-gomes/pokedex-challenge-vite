<script lang='ts'>
import axios from 'axios'
import PokemonEntry from './PokemonEntry.vue'
import SearchBar from './SearchBar.vue'
import ModalWrapper from './ModalWrapper.vue'

export default {
  name: 'PokemonList',
  data() {
    return {
      typePokemonList: [],
      offset: 0,
      limit: 25,
      searchResults: [],
      selectedPokemon: [],
      selectedPokemonSprite: '',
      modalOpen: false,
      currentInput: null,
    }
  },
  components: {
    PokemonEntry,
    SearchBar,
    ModalWrapper,
  },
  computed: {
    computedPokemonList() {
      return this.typePokemonList.slice(this.offset, this.offset + this.limit)
    },
    pokemonListHasData() {
      return this.pokemonList.length !== 0
    },
    searchResultsHasData() {
      return this.searchResults.length !== 0
    },
    inputIsEmpty() {
      return this.currentInput === 'string' && this.currentInput.length === 0 || this.currentInput === null
    },
    previousPageNotPossible() {
      return this.offset < 25
    },
    nextPageNotPossible() {
      return (
        this.offset + this.limit >
        (this.searchResultsHasData
          ? this.searchResults.length
          : this.pokemonList.length)
      )
    },
  },
  props: {
    pokemonList: {
      type: Array,
      default: null,
    },
  },
  created() { },
  watch: {
    pokemonList(newList, oldList) {
      if (newList !== oldList) {
        this.searchResults = []
        this.getPokemonInfo()
      }
    },
  },
  methods: {
    async getPokemonInfo() {
      this.typePokemonList.splice(0)

      if (this.pokemonListHasData) {
        this.pokemonList.forEach(async (entry) => {
          try {
            const pokemonInfo = await axios.get(entry.pokemon.url)
            console.log(pokemonInfo)
            this.typePokemonList.push({
              name: entry.pokemon.name,
              url: pokemonInfo.data.sprites.other.showdown.front_default,
              types: pokemonInfo.data.types,
              number: pokemonInfo.data.order
            })
          } catch (error) {
            console.log('FETCH POKEMONS INFO REQUEST - ERROR:', error)
          }
        })
      }
    },
    nextPage() {
      if (!this.nextPageNotPossible) {
        this.offset = this.offset + 25
      }
    },
    previousPage() {
      if (!this.previousPageNotPossible) {
        this.offset = this.offset - 25
      }
    },
    filterPokemonsList(input) {
      if (input) {
        const filteredPokemons = this.typePokemonList.filter((pokemon) => {
          return String(pokemon.name).includes(input.trim())
        })

        this.searchResults = filteredPokemons
        this.currentInput = input
      } else {
        this.searchResults = []
        this.currentInput = null
      }
    },
    openModal(pokemon) {
      this.selectedPokemon = pokemon.name
      this.selectedPokemonSprite = pokemon.url
      this.modalOpen = true
    },
    closeModal() {
      this.selectedPokemon = ''
      this.selectedPokemonSprite = ''
      this.modalOpen = false
    },
  },
}
</script>

<template>
  <div>
    <SearchBar class='mb-8' v-if='pokemonListHasData' :pokemon-list='pokemonList'
      @input-detected='filterPokemonsList' />
    <div class='flex flex-row flex-wrap gap-4 mb-5'>
      <button
        class='flex flex-col justify-between w-[15.6rem] h-80 bg-white border border-gray-200 rounded-lg shadow-sm dark:bg-gray-800 dark:border-gray-700'
        v-for='pokemon in searchResultsHasData || !inputIsEmpty
          ? searchResults.slice(offset, offset + limit)
          : computedPokemonList' :key='pokemon.name' @click='openModal(pokemon)'>
        <PokemonEntry :pokemon='pokemon' />
      </button>
      <div v-if='!inputIsEmpty && !searchResultsHasData'>
        No results were found for {{ currentInput }}
      </div>
    </div>
    <div v-if='typePokemonList.length > 25'>
      <button
        class='text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:border-gray-700 disabled:bg-gray-100 disabled:text-gray-300 hover:disabled:bg-gray-100 hover:disabled:text-gray-300 disabled:cursor-not-allowed dark:disabled:bg-gray-800 dark:disabled:text-gray-600 dark:hover:disabled:bg-gray-800 dark:hover:disabled:text-gray-600'
        @click='previousPage' :disabled='previousPageNotPossible'>
        Previous page
      </button>
      <button
        class='text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:border-gray-700 disabled:bg-gray-100 disabled:text-gray-300 hover:disabled:bg-gray-100 hover:disabled:text-gray-300 disabled:cursor-not-allowed dark:disabled:bg-gray-800 dark:disabled:text-gray-600 dark:hover:disabled:bg-gray-800 dark:hover:disabled:text-gray-600'
        @click='nextPage' :disabled='nextPageNotPossible'>
        Next page
      </button>
    </div>
    <ModalWrapper v-if='modalOpen' :pokemon='selectedPokemon' :sprite='selectedPokemonSprite'
      @modal-close='closeModal' />
  </div>
</template>
