<template>
    <span>
        <div v-for="row in parseInt(height)" v-bind:key="row" class="row">
            <span class="dot" :class="{ 'active': pixels[row - 1] == 1 }"></span>
        </div>
    </span>
</template>

<script>
export default {
  name: 'LedCol',
  props: {
    pixels: Array,
    height: Number
  }
}
</script>

<style scoped>
.dot {
  width: 4px;
  height: 4px;
  border-radius: 4px;
  background-color: #422;
  display: inline-block;
  margin: 2px;
}
.dot.active {
  background-color: red;
}
</style>
