<template>
  <div id="app">
    <h1>Hex Date Picker</h1>
    <h2>Simple</h2>
    <div style="width: 200px">
      <hex-date-picker input="one" input-display="oneDisplay"></hex-date-picker>
      <input type="text" ref="one" name="date">
      <input type="text" ref="oneDisplay">
    </div>

    <h2>Default picked date</h2>
    <div style="width: 200px">
      <hex-date-picker picked="2015-12-30" input="two" input-display="twoDisplay"></hex-date-picker>
      <input type="text" ref="two">
      <input type="text" ref="twoDisplay">
    </div>

    <h2>Disabling</h2>
    <h3>Dates</h3>
    <div style="width: 200px">
      <hex-date-picker
        picked        = "2015-12-30"
        disable-dates = "2015-12-24|2015-12-25|2015-12-31"
        input         ="three"
        input-display ="threeDisplay"
      ></hex-date-picker>
      <input type="text" ref="three">
      <input type="text" ref="threeDisplay">
    </div>

    <h3>Days of the week</h3>
    <div style="width: 200px">
      <hex-date-picker disable-days="Saturday|Sunday" input="four" input-display="fourDisplay"></hex-date-picker>
      <input type="text" ref="four">
      <input type="text" ref="fourDisplay">
    </div>

    <h3>Past</h3>
    <div style="width: 200px">
      <hex-date-picker disable-past input="five" input-display="fiveDisplay"></hex-date-picker>
      <input type="text" ref="five">
      <input type="text" ref="fiveDisplay">
    </div>

    <h3>Future</h3>
    <div style="width: 200px">
      <hex-date-picker disable-future input="six" input-display="sixDisplay"></hex-date-picker>
      <input type="text" ref="six">
      <input type="text" ref="sixDisplay">
    </div>
  </div>
</template>




<script>
import HexDatePicker from './components/HexDatePicker.vue'

export default {
  name: 'app',
  components: {
    HexDatePicker
  },
}
</script>




<style>
  * + * {
    margin-top: 20px;
  }

  body {
    font-family: Helvetica, sans-serif;
  }

  .calendar .controls {
    display: flex;
    justify-content: center;
  }

  .calendar .controls div {
    display: flex;
    flex-basis: 50%;
    justify-content: space-between;
    padding-left: 5px;
    padding-right: 5px;
  }

  .calendar .controls div span {
    text-align: center;
  }

  .calendar-header {
    display: flex;
    flex-direction: column;
  }

  .days-of-week {
    font-weight: bold;
  }

  .days-of-week,
  .week {
    display: flex;
    margin-top: 0;
  }

  .days-of-week div,
  .day {
    flex-basis: calc(7 / 12 * 100%);
    padding: 5px;
    text-align: center;
  }

  .calendar .controls div,
  .calendar .controls div * + *,
  .days-of-week div,
  .day {
    margin-top: 0;
  }

  .day {
    border-radius: 50%;
  }

  .day:hover {
    background-color: #eee;
  }

  .day:hover {
    cursor: pointer;
  }

  .sr-only {
    border: 0;
    clip: rect(0, 0, 0, 0);
    height: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute;
    width: 1px;
  }

  .in-inactive-month {
    color: #ccc;
  }

  .picked {
    background-color: #327952;
    color: #fff;
  }

  .picked:hover {
    background-color: #327952;
  }

  .today {
    border: 1px solid #888;
  }

  .disabled {
    color: #eee;
  }

  .disabled:hover {
    background-color: inherit;
    cursor: not-allowed;
  }
</style>
