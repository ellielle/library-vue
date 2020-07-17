<template>
  <div id="app">
    <library-main></library-main>
  </div>
</template>

<script>
import LibraryMain from "./components/LibraryMain.vue";
export default {
  name: "App",
  components: {
    "library-main": LibraryMain
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  margin-top: 60px;
}
body {
  background-color: #2c3e50;
}
hr {
  color: #212e3c;
}
</style>
