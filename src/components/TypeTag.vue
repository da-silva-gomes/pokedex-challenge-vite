<script lang='ts'>
export default {
  name: 'TagType',
  data() {
    return {
      typeColors: {
        dragon: 'bg-dragon hover:bg-dragon-hover',
        fighting: 'bg-fighting hover:bg-fighting-hover',
        ghost: 'bg-ghost hover:bg-ghost-hover',
        ice: 'bg-ice hover:bg-ice-hover',
        psychic: 'bg-psychic hover:bg-psychic-hover',
        water: 'bg-water hover:bg-water-hover',
        dark: 'bg-dark hover:bg-dark-hover',
        fairy: 'bg-fairy hover:bg-fairy-hover',
        flying: 'bg-flying hover:bg-flying-hover',
        ground: 'bg-ground hover:bg-ground-hover',
        poison: 'bg-poison hover:bg-poison-hover',
        steel: 'bg-steel hover:bg-steel-hover',
        bug: 'bg-bug hover:bg-bug-hover',
        electric: 'bg-electric hover:bg-electric-hover',
        fire: 'bg-fire hover:bg-fire-hover',
        grass: 'bg-grass hover:bg-grass-hover',
        normal: 'bg-normal hover:bg-normal-hover',
        rock: 'bg-rock hover:bg-rock-hover',
      }
    }
  },
  props: {
    type: {
      type: String as () => 'dragon' | 'fighting' | 'ghost' | 'ice' | 'psychic' | 'water' | 'dark' | 'fairy' | 'flying' | 'ground' | 'poison' | 'steel' | 'bug' | 'electric' | 'fire' | 'grass' | 'normal' | 'rock', // Define the valid types
      default: 'normal',
    },
  },
  computed: {},
};
</script>

<template>
  <div
    class='text-white focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-4 min-w-14 capitalize'
    :class='typeColors[type]'>{{ type }}</div>
</template>
