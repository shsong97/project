<template>
  <GeoView />
</template>

<script>
import GeoView from './components/GeoView.vue'
// import Home from './components/Home.vue';

export default {
  name: 'App',
  components: {
    GeoView,
    // Home,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
