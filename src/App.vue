<template>
  <div id="app">
    <div class="container">
       <Year/>
      <div v-for="(month, index) in monthToRender" :key="index"> Mesi selezionati: {{month}}</div>
    </div>
  </div>
</template>

<script>
import Year from './components/Year.vue'

export default {
  name: 'App',
  components: {
    Year,
  },
  data: function() {
    return {
      monthToRender: []
    }
  }
}
</script>

<style lang="scss">
@import "./scss/_commons.scss";
@font-face {
  font-family: "Inter";
  src: local("Inter"),
  url(./font/Inter/Inter-Regular.ttf) format("truetype");
}
#app {
  .container {
    height: 90vh;
    @include flex-column;
    @include flex-center;
  }
}
</style>
