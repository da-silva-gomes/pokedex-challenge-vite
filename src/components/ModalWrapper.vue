<script lang='ts'>
import axios from 'axios';
import { defineComponent } from "vue";


const POKEMON_DETAILS_API_URL = `https://pokeapi.co/api/v2/pokemon/`;
const POKEMON_FLAVOR_API_URL = `https://pokeapi.co/api/v2/pokemon-species/`;

export default defineComponent({
  name: 'ModalWrapper',
  data() {
    return {
      details: {
        order: '',
        types: [] as Array<{ type: { name: string } }>,
        height: 0,
        weight: 0,
        abilities: [] as Array<{ ability: { name: string } }>
      },
      flavor: null
    };
  },
  props: {
    pokemon: {
      type: String,
      default: null,
    },
    sprite: {
      type: String,
      default: null,
    },
  },
  computed: {},
  created() {
    this.getPokemonDetails();
  },
  watch: {
    pokemon(newPokemon, oldPokemon) {
      if (newPokemon !== oldPokemon) {
        this.getPokemonDetails();
      }
    },
  },
  methods: {
    async getPokemonDetails() {
      try {
        const pokemonDetails = await axios.get(
          `${POKEMON_DETAILS_API_URL}${this.pokemon}`
        );

        this.details = pokemonDetails.data;
      } catch (error) {
        console.log('FETCH POKEMON DETAILS REQUEST - ERROR:', error);
      }

      try {
        const pokemonFlavor = await axios.get(
          `${POKEMON_FLAVOR_API_URL}${this.pokemon}`
        );

        this.flavor = pokemonFlavor.data.flavor_text_entries[0].flavor_text.replace(/(\r\n|\n|\r)/gm, ' ')
      } catch (error) {
        console.log('FETCH POKEMON FLAVOR REQUEST - ERROR:', error);
      }
    },
  },
});
</script>

<template>
  <div class="modal-mask fixed z-9998 top-0 left-0 w-full h-full bg-black/50">
    <div
      class='overflow-y-auto overflow-x-hidden fixed top-20 right-0 left-0 z-50 justify-center items-center w-full md:right-0 h-[calc(100%-1rem)] max-h-full'
      @click='$emit("modal-close")'>
      <div class='relative p-4 w-full max-w-2xl max-h-full mx-auto' @click.stop=''>
        <div class="relative bg-white rounded-lg shadow-sm dark:bg-gray-700">
          <div class='flex flex-col justify-between p-4 md:p-5 space-y-4'>
            <div class="flex flex-row items-center">
              <div class="flex flex-row items-center">
                <p class='font-normal text-gray-700 dark:text-gray-400 mr-3'>#{{ details.order }}</p>
                <h5 class='text-2xl font-bold tracking-tight text-gray-900 dark:text-white capitalize'>{{ pokemon }}
                </h5>
              </div>
              <button type="button"
                class="absolute top-3 end-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
                @click='$emit("modal-close")'>
                <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                  viewBox="0 0 14 14">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                </svg>
                <span class="sr-only">Close modal</span>
              </button>
            </div>
            <div class="flex flex-row">
              <div class="flex flex-row justify-center w-3/5">
                <img class='h-70 p-5 mt-10 mr-10 rounded-t-lg object-contain' :src='sprite' alt='' />
              </div>
              <dl class="flex flex-col w-2/5 max-w-md text-gray-900 dark:text-white">
                <div class='flex flex-col mb-3'>
                  <dt class="mb-2 text-gray-500 md:text-lg dark:text-gray-400">Types</dt>
                  <dd v-for="entry in details.types" :key="entry.type.name" :class="`bg-${entry.type.name}`"
                    class='flex justify-center w-20 text-white text-xs font-semibold px-2.5 py-0.5 rounded-sm capitalize last:mt-1'>
                    {{
                      entry.type.name
                    }}</dd>
                </div>
                <div class="flex flex-col my-3">
                  <dt class="mb-1 text-gray-500 md:text-lg dark:text-gray-400">Height</dt>
                  <dd class="text-lg font-semibold">{{ (details.height / 10).toFixed(1) }} m</dd>
                </div>
                <div class="flex flex-col my-3">
                  <dt class="mb-1 text-gray-500 md:text-lg dark:text-gray-400">Weight</dt>
                  <dd class="text-lg font-semibold">{{ (details.weight / 10).toFixed(1) }} kg</dd>
                </div>
                <div class="flex flex-col justify-center mt-3">
                  <dd class="mb-1 text-gray-500 md:text-lg dark:text-gray-400">Abilities</dd>
                  <dd v-for="entry in details.abilities" :key="entry.ability.name"
                    class="text-lg font-semibold capitalize">{{
                      entry.ability.name }}</dd>
                </div>
              </dl>
            </div>
            <div>
              <span class="mb-1 text-gray-500 md:text-lg dark:text-gray-400">Pokedex entry</span>
              <p class="text-black dark:text-white text-lg font-semibold">{{ flavor }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
