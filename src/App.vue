<template>
  <div id="app">
    <LedSign :message="message" :width="192" :height="20" :scrollMode="scrollMode" :align="alignment" :scrollSpacing="parseInt(scrollSpacing, 10)" />

    <hr>

    <div>
      <h2>Configuration</h2>
  
      Message:<br>
      <input type="text" v-model="message">

      <br>

      ScrollMode:<br>
      <label>
        <input type="radio" value="auto" v-model="scrollMode">
        Auto
      </label>

      <label>
        <input type="radio" value="scroll" v-model="scrollMode">
        Scroll
      </label>

      <label>
        <input type="radio" value="none" v-model="scrollMode">
        Do not scroll
      </label>

      <br>

      ScrollSpacing:<br>
      <input type="text" v-model="scrollSpacing">

      <br>

      Alignment:<br>

      <label>
        <input type="radio" value="left" v-model="alignment">
        Left
      </label>

      <label>
        <input type="radio" value="center" v-model="alignment">
        Center
      </label>

      <label>
        <input type="radio" value="right" v-model="alignment">
        Right
      </label>
    </div>

    <hr>

    <div>
        <CharCreate :width="20" :height="20"></CharCreate>
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
      message: 'WELCOME',
      scrollMode: 'auto',
      alignment: 'center',
      scrollSpacing: 192
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
