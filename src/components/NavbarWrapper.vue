<script lang='ts'>
import LogoWrapper from './LogoWrapper.vue'
import Github from './icons/IconGithub.vue'

export default {
  name: 'NavbarWrapper',
  components: {
    LogoWrapper,
    Github,
  },
};
</script>

<template>
  <div class='bg-white dark:bg-gray-900 fixed w-full z-20 top-0 start-0 border-b border-gray-200 dark:border-gray-600'>
    <div class='max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4'>
      <LogoWrapper />
      <a href='#' class='flex items-center space-x-3 rtl:space-x-reverse'>
        <Github />
      </a>
    </div>
  </div>
</template>
