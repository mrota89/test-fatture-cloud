<template>
  <div id="app">
    <Year/>
  </div>
</template>

<script>
import Year from './components/Year.vue'

export default {
  name: 'App',
  components: {
    Year,
  }
}
</script>

<style>
@font-face {
  font-family: "Inter";
  src: local("Inter"),
  url(./font/Inter/Inter-Regular.ttf) format("truetype");
}
</style>
