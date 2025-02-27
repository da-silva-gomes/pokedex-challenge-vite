<script lang='ts'>
import TypeTag from './TypeTag.vue';

export default {
  name: 'PokemonType',
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
      type: Array,
      default: null,
    },
  },
  created() { },
  methods: {
    selectType(type) {
      this.selectedType = type;
      this.$emit('type-selected', type);
    },
  },
};
</script>

<template>
  <div class='mb-10'>
    <p class='text-center text-sm font-normal text-gray-500 dark:text-gray-400 mb-4'>Select a pokemon type</p>
    <div class='flex flex-row flex-wrap w-6/12 mx-auto justify-center'>
      <!-- if value clicked is selected, change style -->
      <TypeTag v-for='type in types' :key='type.name' :type='type' @click='selectType(type)'
        class='mr-4 mb-2 last:mr-0' />
    </div>
  </div>
</template>
