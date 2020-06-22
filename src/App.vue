<template>
  <div id="app">
    <!-- <router-link to="/">HelloWorld</router-link>|
    <router-link to="/gismap">GisMap</router-link>-->
    <gismap />
  </div>
</template>

<script>
import gismap from './components/gismap.vue';

export default {
  name: 'App',
  components: {
    // HelloWorld,
    gismap
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
  margin-top: 0px;
}
.tygismap {
  width: 100%;
  height: 100%;
}
</style>
