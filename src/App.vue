<template>
  <div id="app">
      <Skills />
  </div>
</template>

<script>
import Skills from './components/Skills'
import GridComponent from './components/Grid'

export default {
  name: 'app',
  components: {
    Skills,
    GridComponent
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Montserrat:400,700');
#app {
  font-family: 'Montserrat', 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 80%;
  margin: auto;
}
</style>
