<template>
  <div id="year" @mousedown="startDrag">
    <Month v-for="(item, index) in ajaxCallData" 
    :month="item.mese" 
    :bill="item.documenti" 
    :revenue="item.importo" 
    :indice="index" 
    :altezza="item.altezza"
    :key="index"/>
  </div>
</template>

<script>
import Month from './Month.vue'

export default {
  name: 'Year',
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
      ajaxCallData: [],
      monthRevenue: [],
      monthIDarray: [],
      altezze: [],
      dragging: false,
      maxRevenue: 0
    }
  },
  mounted: function() {
    //listener sul rilascio del click
    window.addEventListener('mouseup', this.stopDrag);

    //chiamata ajax
    this.axios
    .get('http://staccah.fattureincloud.it/testfrontend/data.json')
    .then((xhr) => {
      this.ajaxCallData = xhr.data.mesi;

      //pusho nell'oggetto  il mese corrispondente
      this.ajaxCallData.forEach((element, index) => {
        this.month.forEach((item, indice) => {
          if(index === indice) {
            element.mese = item;
          }
        });

        //salvo in un array gli importi mensili
        this.monthRevenue.push(element.importo);
      })//end forEach

      //trovo l'importo più alto
      this.maxRevenue = Math.max(...this.monthRevenue);

      //trasformo in percentuale i valori degli importi e li salvo in un array
      this.monthRevenue.forEach((element) => {
        let altezzaRettangolo = 0;
        altezzaRettangolo = element/this.maxRevenue;
        this.altezze.push(parseFloat(altezzaRettangolo.toFixed(2)) * 100);
      })

      this.ajaxCallData.forEach((element, index) => {
        this.altezze.forEach((item, indice) => {
          if(index === indice) {
            element.altezza = item;
          }
        })
      })
    });//end axios call
  },//end mounted

  methods: {
    startDrag() {
      this.dragging = true;
    },
    stopDrag() {
      this.dragging = false;
    },
  }
}
</script>

<style>
#year {
  display: flex;
}
</style>
