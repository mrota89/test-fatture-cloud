<template>
  <div class="frame" @mouseenter="draggedMonth" @mousedown.exact="clickedMonth" @click.ctrl="pushMonth">
    <div class="month">{{ month }}</div>

    <!-- barra grafico di sfondo -->
    <div class="graph" :style="styleObject"></div>
    <!-- /barra grafico di sfondo -->

    <div class="total">
      <div class="amount-bill">{{ bill }} doc.</div>
      <div class="amount-revenue">{{ revenue }} €</div>

      <!-- selettore casella -->
      <div :class="selectedMonth()"></div>
      <!-- /selettore casella -->
    </div>
  </div>
</template>

<script>
export default {
  name: 'Month',
  props: {
    month: String,
    bill: Number,
    revenue: Number,
    indice: Number,
    graph: Number,
  },
  
  data: function() {
    return {
       //dati di stile per la barra-grafico di sfondo
      styleObject: {
        height: `calc(85px * ${this.graph})`,
        backgroundColor: '#e0f1eb', 
      }
    }
  },

  methods: {
    //a seconda dell'evento click, ritorna una classe diversa per la barra di selezione
    selectedMonth: function() {
      if (this.$parent.monthIDarray.includes(this.indice) && this.$parent.dragging) {
        return 'selector-bar selected-clicked'
      } else if (this.$parent.monthIDarray.includes(this.indice) && !this.$parent.dragging) {
        return 'selector-bar selected'
      } else {
        return 'selector-bar'
      }
    },
    //salva in un array l'index del mese selezionato
    pushMonth: function() {
      this.$parent.monthIDarray.push(this.indice);
      
      if(!this.$parent.monthNameArray.includes(this.month)) {
        this.$parent.monthNameArray.push(this.month);
      }
    },
    clickedMonth: function() {
      this.$parent.monthIDarray.length = 0;
      this.$parent.monthNameArray.length = 0;
      this.pushMonth();
    },
    draggedMonth: function() {
      if(this.$parent.dragging) {
        this.pushMonth();
      }
    },
  }
}
</script>

<style lang="scss" scoped>

/*variabili */
$border-grey: solid 1px #cecece;
$light-blue: #0d97d5;
$border-blue: solid 2px $light-blue;
$green-select: #00875A;
$green-clicked: #80ad9d;
$num-doc-color: #6F7E86;
$frame-width: 80px;
$frame-height: 110px;
$month-height: 25px;
$total-height: calc(110px - 25px);
$amount-height: 18px;
$pad-10-l: 10px;
/*/variabili */

.frame {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  border-right: $border-grey;
  border-bottom: $border-blue;
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  width: $frame-width;
  height: $frame-height;
  font-weight: 500;
  position:relative;
  
  .month {
    height: $month-height;
    line-height: $month-height;
    border-top: $border-grey;
    border-bottom: $border-grey;
    padding-left: $pad-10-l;
    color: $light-blue;
  }

  .total {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    height: $total-height;
    width: $frame-width;
    cursor: pointer;
    position: absolute;
    transform: translate( 0, -50%);
    top: 70%;

    .amount-bill, .amount-revenue {
      height: $amount-height;
      line-height: $amount-height;
      color: $num-doc-color;
      padding-left: $pad-10-l;
    }

    .amount-revenue {
      color: $green-select;
      margin-bottom: 5px;
    }

    .selector-bar {
      height: 8px;
    }

    .selector-bar.selected-clicked {
      background: $green-clicked;
    }

    .selector-bar.selected {
      background: $green-select;
    }
  }
}
.frame:first-child {
  border-left: $border-grey;
}
</style>
