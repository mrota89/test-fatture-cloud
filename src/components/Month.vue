<template>
  <div class="frame" @mouseenter="draggedMonth" @mousedown="clickedMonth">
    <div class="month">{{ month }}</div>
    <div class="total">
      <div class="amount-bill">{{ bill }} doc.</div>
      <div class="amount-revenue">{{ revenue }} €</div>
      <div :class="selectedMonth()"></div>
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
    altezza: Number
  },
  methods: {
    selectedMonth: function() {
      if (this.$parent.monthIDarray.includes(this.indice)) {
        return 'selector-bar selected'
      } else {
        return 'selector-bar'
      }
    },
    pushMonth: function() {
      this.$parent.monthIDarray.push(this.indice);
    },
    clickedMonth: function() {
      this.$parent.monthIDarray = [];
      this.pushMonth();
    },
    draggedMonth: function() {
      if(this.$parent.dragging) {
        this.pushMonth();
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.frame {
  border-right: solid 1px #cecece;
  border-bottom: solid 2px #0D97D5;
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  width: 80px;
  height: 110px;
  font-weight: 500;
  
  .month {
    height: 25px;
    line-height: 25px;
    border-top: solid 1px #cecece;
    border-bottom: solid 1px #cecece;
    padding-left: 10px;
    color: #0d97d5;
  }

  .total {
    height: calc(100% - 25px);
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    cursor: pointer;

    .amount-bill, .amount-revenue {
      height: 18px;
      line-height: 18px;
      color: #6F7E86;
      padding-left: 10px;
    }

    .amount-revenue {
      color: #00875A;
    }

    .selector-bar {
      height: 8px;
    }

    .selector-bar.selected {
      background: #00875A;
    }
  }
}
.frame:first-child {
  border-left: solid 1px #cecece;
}
</style>
