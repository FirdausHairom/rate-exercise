<template>
  <div id="app">
    <Rates msg="Rates Exercise"/>
  </div>
</template>

<script>

import Rates from './components/Rates.vue'

export default {
  name: 'App',
  components: {
    Rates
  }
}
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
