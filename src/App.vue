<template>
  <div id="app">
    <advThree />
  </div>
</template>

<script>
import threeLyfe from '@/components/threeLyfe.vue'
import advThree from '@/components/advThree.vue'

export default {
  name: 'App',
  components: {
    threeLyfe,
    advThree
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
