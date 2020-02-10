<template>
  <div id="app">
  <generator/>
  </div>
</template>

<script>
import generator from "./components/generator.vue";

export default {
  name: "app",
  components: {
    generator
  }
};
</script>

<style lang="stylus">
<link href="//maxcdn.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css" rel="stylesheet">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
</style>
