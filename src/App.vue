<template>
  <div id="app">
    <LedSign :message="message" :width="128" :height="7" />

    <hr>

    <input type="text" v-model="message">

    <hr>

    <div>
        <CharCreate :width="5" :height="7"></CharCreate>
    </div>
  </div>
</template>

<script>
import LedSign from './components/LedSign.vue'
import CharCreate from './components/CharCreate.vue'

export default {
  name: 'App',
  components: {
    LedSign,
    CharCreate
  },
  data() {
    return {
      message: 'WELCOME'
    }
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
