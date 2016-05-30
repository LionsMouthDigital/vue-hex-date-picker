<template>
  <div id="app">
    <hex-date-picker v-ref:one></hex-date-picker>
    <input type="text" v-model="$refs.one.selected">
    <input type="text" v-model="$refs.one.displayDate">

    <hex-date-picker picked="2015-12-31" v-ref:two></hex-date-picker>
    <input type="text" v-model="$refs.two.selected">
    <input type="text" v-model="$refs.two.displayDate">
  </div>
</template>

<script>
  import HexDatePicker from './components/HexDatePicker.vue'

  export default {
    components: {
      HexDatePicker
    }
  }
</script>
