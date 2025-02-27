<script lang='ts'>
export default {
  name: 'PokemonType',
  components: {},
  props: {
    pokemon: {
      type: Object,
      default: () => { },
    },
  },
};
</script>

<template>
  <img class='h-50 mx-auto p-5 rounded-t-lg' :src='pokemon.url' alt='' />
  <div class='flex flex-col items-start p-5'>
    <p class='mb-1 font-normal text-gray-700 dark:text-gray-400'>#{{ pokemon.number }}</p>
    <h5 class='mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white capitalize'>{{ pokemon.name }}
    </h5>
    <div class='flex flex-row'>
      <span v-for="entry in pokemon.types" :key="entry.type.name" :class="`bg-${entry.type.name}`"
        class='text-white text-xs font-semibold px-2.5 py-0.5 rounded-sm capitalize'>{{ entry.type.name }}</span>
    </div>
  </div>
</template>
