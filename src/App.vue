<template>
  <div id="app">
    <LedSign msg="Welcome to Your Vue.js App" :width="128" :height="8" />
  </div>
</template>

<script>
import LedSign from './components/LedSign.vue'

export default {
  name: 'App',
  components: {
    LedSign
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
