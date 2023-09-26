<template>
  <BackgroundThreeD
    object-path="deer.gltf"
    :object-position="[0, -1.5, -1.5]"
  />
</template>

<script>
import BackgroundThreeD from "./components/BackgroundThreeD.vue";

export default {
  name: "App",
  components: {
    BackgroundThreeD,
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
  margin-top: 60px;
}
</style>
