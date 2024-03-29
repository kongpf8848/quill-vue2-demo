<template>
  <div id="app">
    <RichEditor msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import RichEditor from "./components/RichEditor.vue";

export default {
  name: "App",
  components: {
    RichEditor,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
