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
      this.createArray(this.month, 'mese');

        //salvo in un array gli importi mensili
      this.ajaxCallData.forEach((element) => {
        this.monthRevenue.push(element.importo);
      })
        
      //trovo l'importo più alto
      this.maxRevenue = Math.max(...this.monthRevenue);

      //trasformo in percentuale i valori degli importi e li salvo in un array
      this.monthRevenue.forEach((element) => {
        let altezzaRettangolo = 0;
        altezzaRettangolo = element/this.maxRevenue;
        this.altezze.push(parseFloat(altezzaRettangolo.toFixed(2)) * 100);
      })

      this.createArray(this.altezze, 'altezza');
    });//end axios call
  },//end mounted

  methods: {
    startDrag: function() {
      this.dragging = true;
    },
    stopDrag: function() {
      this.dragging = false;
    },
    createArray: function(array, property) {
      this.ajaxCallData.forEach((element, index) => {
        array.forEach((item, indice) => {
          if(index === indice) {
            element[property] = item;
          }
        })
      })   
    }
  }//end methods
}
</script>

<style>
#year {
  display: flex;
}
</style>
