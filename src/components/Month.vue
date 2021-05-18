<template>
  <div :class="monthSelected()" @click="flag=true">
    <div class="month">{{ month }}</div>
    <div class="total">
      <div class="amount-bill">{{ bill }} doc.</div>
      <div class="amount-revenue">{{ revenue }} €</div>
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
    flag: Boolean
  },
  methods: {
    monthSelected: function() {
      if (this.flag) {
        return 'frame selected'
      } else {
        return 'frame'
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
    padding-left: 10px;
    cursor: pointer;

    .amount-bill, .amount-revenue {
      height: 18px;
      line-height: 18px;
      color: #6F7E86;
    }

    .amount-revenue {
      padding-bottom: 10px;
      color: #00875A;
    }
  }
}
.frame:first-child {
  border-left: solid 1px #cecece;
}

.frame.selected {
  border-bottom: solid 5px #0D97D5;
}
</style>
