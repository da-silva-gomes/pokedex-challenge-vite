<script setup lang='ts'>
import NavbarWrapper from './components/NavbarWrapper.vue'
import ListWrapper from './components/ListWrapper.vue'
import FooterWrapper from './components/FooterWrapper.vue'
</script>

<template>
  <header class='h-16 mb-20'>
    <NavbarWrapper />
  </header>

  <main class='flex-1'>
    <ListWrapper />
  </main>

  <footer>
    <FooterWrapper />
  </footer>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
