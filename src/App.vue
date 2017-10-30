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

$title-color: #e80404;

body {
  font-family: 'Courier', monospace;
  text-align: center;
  color: #fff;
  background: #222;
}

@font-face {
  font-family: speed;
  src: url(./assets/speed.ttf);
}

.correct {
  background: #79d64a;
  height: 25px;
  width: 12px;
  vertical-align: top;
  display: inline-block;
}

.wrong {
  background: $title-color;
  height: 25px;
  width: 12px;
  vertical-align: top;
  display: inline-block;
}

h1 {
  font-family: speed, sans-serif;
  color: $title-color;
  font-size: 40px;
  margin: 0;
  padding: 0;
}

h2 {
  font-family: speed, sans-serif;
  color: $title-color;
  font-size: 40px;
  margin: 0;
  padding: 0;
}

</style>
