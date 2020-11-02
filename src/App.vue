<template>
  <div id="app">
    <mainComponent />
  </div>
</template>

<script>
import mainComponent from "./components/mainComponent.vue";

export default {
  name: "App",
  components: {
    mainComponent
  }
};
</script>

<style lang="scss">
#app {
  @import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap");
  $font: "Roboto", sans-serif;
}
</style>
