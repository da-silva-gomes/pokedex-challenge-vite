<script lang='ts'>
import TypeTag from './TypeTag.vue';

interface PokemonType {
  name: 'dragon' | 'fighting' | 'ghost' | 'ice' | 'psychic' | 'water' | 'dark' | 'fairy' | 'flying' | 'ground' | 'poison' | 'steel' | 'bug' | 'electric' | 'fire' | 'grass' | 'normal' | 'rock';
}

export default {
  name: 'TypesList',
  data() {
    return {
      selectedType: '',
    };
  },
  components: {
    TypeTag,
  },
  props: {
    types: {
      type: Array as () => PokemonType[],
      default: null,
    },
  },
  created() { },
  methods: {
    selectType(type: PokemonType) {
      this.selectedType = type.name;
      this.$emit('type-selected', type);
    },
  },
};
</script>

<template>
  <div class='mb-10'>
    <p class='text-center text-sm font-normal text-gray-500 dark:text-gray-400 mb-4'>Select a pokemon type</p>
    <div class='flex flex-row flex-wrap w-6/12 mx-auto justify-center'>
      <TypeTag v-for='type in types' :key='type.name' :type='type.name' @click='selectType(type)'
        class='mr-4 mb-2 last:mr-0' />
    </div>
  </div>
</template>
