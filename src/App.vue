<template>
  <div id="app">
    <Front/>
  </div>
</template>

<script>
import Front from './components/Front'

export default {
  name: 'App',
  components: {
    Front,
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
  /* margin-top: 60px; */
}
</style>
