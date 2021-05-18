<template>
  <div class="frame">
    <div class="month">{{ month }}</div>
    <div class="total">
      <div class="n-bill"></div>
      <div class="revenue"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Month',
  props: {
    month: String
  }
}
</script>

<style lang="scss" scoped>

</style>
