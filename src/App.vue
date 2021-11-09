<template>
  <div id="app">
   <Select />
  </div>
</template>

<script>
import Select from './components/Select.vue';

export default {
  name: 'App',
  components: {
    Select
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

input, button {
  outline: none;
  cursor: pointer;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-family: 'Noto Sans', sans-serif;
  font-size: 16px;
  line-height: 1.5;
}
</style>
