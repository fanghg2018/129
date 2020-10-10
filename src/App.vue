<template>
  <div id="app">
    <Voice />
  </div>
</template>

<script>
import Voice from './views/Voice'

export default {
  name: 'app',
  components: {
    Voice
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
