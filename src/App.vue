<template>
  <div id="app">
    <MovieIndex />
  </div>
</template>

<script>
import MovieIndex from "./components/MovieIndex.vue";

export default {
  name: "App",
  components: {
    MovieIndex,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px; */
}
</style>
