<template>
  <div id="app">
    <cards />
  </div>
</template>

<script>
import cards from './Cards.vue'

export default {
  name: 'app',
  components: {
    cards
  },
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #F06292;

  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

body {
  margin: 0;
}

*,
*:before,
*:after {
  box-sizing: border-box;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
