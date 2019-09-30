<template>
  <div id="app">
    <v-parallax dark src="./assets/piano2.jpg" height="1300">
        <h1>Song Key Finder</h1>
        <div style="margin-top: 100px;"> </div>
        <Upload />
    </v-parallax>
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
}


</style>
