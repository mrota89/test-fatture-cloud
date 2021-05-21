<template>
  <div id="app">
    <div class="container">
      <Year/>
      <div v-if="monthToRender.length > 0" class="show-month">
        <div>Mesi selezionati:</div>
        <div v-for="(month, index) in monthToRender" :key="index">     
          <div>{{month}}</div>
        </div>
      </div>
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
@font-face {
  font-family: "Inter";
  src: local("Inter"),
  url(./font/Inter/Inter-Regular.ttf) format("truetype");
}
#app {
  .container {
    height: 90vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .show-month {
      height: 50%;
      padding-top: 10px;
      position: absolute;
      transform: translate(-50% , -50%);
      top: 80%;
      left: 50%;
    }
  }
}
</style>
