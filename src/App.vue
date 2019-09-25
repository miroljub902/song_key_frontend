<template>
  <div id="app">
    <h1>Song Key Finder</h1>
    <img alt="Vue logo" src="./assets/1.jpeg" />
    <div style="margin-top: 100px;"> </div>
    <Upload />
  </div>
</template>

<script>
import Upload from "./components/Upload.vue";

export default {
  name: "app",
  components: {
    Upload
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
