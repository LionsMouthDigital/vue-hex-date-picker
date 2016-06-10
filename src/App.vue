<template>
  <div id="app">
    <h1>Hex Date Picker</h1>
    <h2>Simple</h2>
    <hex-date-picker v-ref:one></hex-date-picker>
    <input type="text" v-model="$refs.one.selected">
    <input type="text" v-model="$refs.one.displayDate">

    <h2>Default picked date</h2>
    <hex-date-picker picked="2015-12-30" v-ref:two></hex-date-picker>
    <input type="text" v-model="$refs.two.selected">
    <input type="text" v-model="$refs.two.displayDate">

    <h2>Disabling</h2>
    <h3>Dates</h3>
    <hex-date-picker
      picked        = "2015-12-30"
      disable-dates = "2015-12-24|2015-12-25|2015-12-31"
      v-ref:three
    ></hex-date-picker>
    <input type="text" v-model="$refs.three.selected">
    <input type="text" v-model="$refs.three.displayDate">

    <h3>Days of the week</h3>
    <hex-date-picker disable-days="Saturday|Sunday" v-ref:four></hex-date-picker>
    <input type="text" v-model="$refs.four.selected">
    <input type="text" v-model="$refs.four.displayDate">

    <h3>Past</h3>
    <hex-date-picker disable-past v-ref:five></hex-date-picker>
    <input type="text" v-model="$refs.five.selected">
    <input type="text" v-model="$refs.five.displayDate">

    <h3>Future</h3>
    <hex-date-picker disable-future v-ref:six></hex-date-picker>
    <input type="text" v-model="$refs.six.selected">
    <input type="text" v-model="$refs.six.displayDate">
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
