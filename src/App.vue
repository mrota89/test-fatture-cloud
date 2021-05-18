<template>
  <div id="app">
    <Month v-for="(item, index) in revenueData" :month="item.mese" :bill="item.documenti" :revenue="item.importo" :key="index"/>
  </div>
</template>

<script>
import Month from './components/Month.vue'

export default {
  name: 'App',
  components: {
    Month,
  },
  data: function(){
    return {
      month: 
      [
        'Gennaio', 
        'Febbraio', 
        'Marzo', 
        'Aprile', 
        'Maggio', 
        'Giugno', 
        'Luglio', 
        'Agosto', 
        'Settembre', 
        'Ottobre', 
        'Novembre', 
        'Dicembre'
      ],
      revenueData: []
    }
  },
  mounted: function() {
    this.axios
    .get('http://staccah.fattureincloud.it/testfrontend/data.json')
    .then((xhr) => {
      this.revenueData = xhr.data.mesi;
      this.revenueData.forEach((element, index) => {
        this.month.forEach((item, indice) => {
          if(index === indice) {
            element.mese = item;
          }
        })
      })//end forEach
    })//end axios call
  }//end mounted
}
</script>

<style>
@font-face {
  font-family: "Inter";
  src: local("Inter"),
  url(./font/Inter/Inter-Regular.ttf) format("truetype");
}

#app {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
</style>
