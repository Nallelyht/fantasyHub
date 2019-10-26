<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'app',
  components: {}
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Nunito:400,700&display=swap");
@import "~/assets/scss/styles.scss";
#app {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  vertical-align: baseline;
  height: 100vh;
}
* {
  font-family: "Nunito", sans-serif;
}
</style>
