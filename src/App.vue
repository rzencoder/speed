<template>
  <div id="app">
    <Main/>
  </div>
</template>

<script>

import Main from './components/main'

export default {
  name: 'app',
  components: {
    Main
  }
}

</script>

<style lang="scss">

body {
  font-family: 'Courier', Helvetica, Arial, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  background: #222;
  
}

@font-face {
    font-family: speed;
    src: url(./assets/speed.ttf);
}

h1 {
  font-family: speed, sans-serif;
  color: red;
  font-size: 40px;
  margin: 0;
  padding: 0px;
}

h2 {
  font-family: speed, sans-serif;
  color: red;
  font-size: 40px;
  margin: 0;
  padding: 0px;
}

</style>
