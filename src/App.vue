<script>
import { RouterLink, RouterView } from 'vue-router'
import Header from './components/Header.vue'
export default {
  data() {
    return {}
  },

  components: {
    Header
  }
}
</script>

<template>
  <Header> </Header>

  <RouterView />
</template>

<style lang="scss">
@use './style/general.scss';
</style>
