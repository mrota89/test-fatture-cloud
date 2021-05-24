<template>
  <div id="year" @mousedown="startDrag" @mouseleave="stopDrag">
    <Month v-for="(item, index) in ajaxCallData" 
    :month="item.mese" 
    :bill="item.documenti" 
    :revenue="item.toStringImport" 
    :indice="index" 
    :graph="item.graphValue"
    :key="index"/>
    <div v-if="error">The page could not be loaded. Please try again later.</div>
  </div>
</template>

<script>
import Month from './Month.vue'

export default {
  name: 'Year',
  components: {
    Month
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
      monthNameArray: [],
      graphValues: [],
      numToStringValue: [],
      dragging: false,
      error: false,
      maxRevenue: 0
    }
  },
  mounted: function() {
    //listener sul rilascio del click
    window.addEventListener('mouseup', this.stopDrag);

    //mostra messaggio di errore in caso di fallita chiamata ajax
    setTimeout(() => {
      this.errorMessage();
    }, 3000);
    
    //chiamata ajax
    this.axios
    .get('http://staccah.fattureincloud.it/testfrontend/data.json')
    .then((xhr) => {
      //salvo in array gli oggetti ottenuti dalla chiamata ajax
      this.ajaxCallData = xhr.data.mesi;

      //pusho nell'oggetto il mese corrispondente
      this.pushProperties(this.month, 'mese');

      //salvo in un array gli importi mensili
      this.ajaxCallData.forEach((element) => {
        this.monthRevenue.push(element.importo);
      })
        
      //trovo l'importo più alto
      this.maxRevenue = Math.max(...this.monthRevenue);

      this.monthRevenue.forEach((element) => {
        //trasformo in percentuale i valori degli importi e li salvo in un array
        let graphHeight = 0;
        graphHeight = element/this.maxRevenue;
        this.graphValues.push(parseFloat(graphHeight.toFixed(3)));

        //trasformo in "dot notation" i valori degli importi e li salvo in un array
        let numberToString = element.toString().replace(/(\d)(?=(\d\d\d)+(?!\d))/g, "$1.");
        this.numToStringValue.push(numberToString);
      })

      //pusho nell'oggetto il valore percentuale corrispondente
      this.pushProperties(this.graphValues, 'graphValue');

      //pusho nell'oggetto la stringa dell'importo corrispondente
      this.pushProperties(this.numToStringValue, 'toStringImport');

    });//end ajax call

    this.$parent.monthToRender = this.monthNameArray;
  },//end mounted

  methods: {
    startDrag: function() {
      this.dragging = true;
    },

    stopDrag: function() {
      this.dragging = false;
    },

    pushProperties: function(array, property) {
      this.ajaxCallData.forEach((element, index) => {
        array.forEach((item, indice) => {
          if(index === indice) {
            element[property] = item;
          }
        })
      })   
    },

    errorMessage: function() { 
      if(this.ajaxCallData.length === 0) {
        this.error = true;
      }
    },
  }//end methods
}
</script>

<style>
#year {
  display: flex;
}
</style>
