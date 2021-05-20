<template>
  <div id="year" @mousedown="startDrag()" @mousemove="doDrag">
    <Month v-for="(item, index) in revenueData" 
    :month="item.mese" 
    :bill="item.documenti" 
    :revenue="item.importo" 
    :indice="index" 
    :key="index"/>
    <div>X: {{x}}, Y: {{y}}</div>
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
      revenueData: [],
      frameIDX: -1,
      isClicked: false,
      dragging: false,
      x: 'no',
      y: 'no',
    }
  },
  mounted: function() {
    window.addEventListener('mouseup', this.stopDrag);
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
  },//end mounted

  methods: {
    startDrag() {
      this.dragging = true;
      this.x = this.y = 0;
    },
    stopDrag() {
      this.dragging = false;
      this.x = this.y = 'no';
    },
    doDrag(event) {
      if (this.dragging) {
        this.x = event.clientX;
        this.y = event.clientY;
      }
    }
  }
}
</script>

<style>
#year {
  display: flex;
}
</style>
