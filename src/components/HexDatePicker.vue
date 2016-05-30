<template>
  <div class="calendar">
    <table>
      <thead>
        <tr>
          <th>
            <button @click="prevMonth()" class="button-prev">
              <span>&lt;</span>
            </button>
          </th>
          <th colspan="2">{{ moment(date).format('MMM') }}</th>
          <th>
            <button @click="nextMonth()" class="button-next">
              <span>&gt;</span>
            </button>
          </th>

          <th>
            <button @click="prevYear()" class="button-prev">
              <span>&lt;</span>
            </button>
          </th>
          <th>{{ moment(date).format('YYYY') }}</th>
          <th>
            <button @click="nextYear()" class="button-next">
              <span>&gt;</span>
            </button>
          </th>
        </tr>

        <tr>
          <th v-for="day in daysOfWeek">{{ day }}</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="week in weeks">
          <td v-for="day in week" @click="pick(day)">
            {{ moment(day, format).format('DD') }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import moment from 'moment';
  import HexCalendar from 'vue-hex-calendar';

  export default {
    extends: HexCalendar,

    data() {
      return {
        // Base date to build the calendar on. If `picked` prop is unspecified, it will default to
        // today, and the rest of the current month will be shown.
        date:     '',
        // Selected date.
        selected: '',
      };
    },


    computed: {
      /**
       * Display dates in a user-friendly format.
       *
       * @author Curtis Blackwell
       * @return {string}
       */
      displayDate() {
        return this.selected ? moment(this.selected).format(this.displayForamt) : '';
      },
    },


    props: {
      // Set the user-friendly date format.
      // See [Moment.js docs](http://momentjs.com/docs/#year-month-and-day-tokens).
      displayForamt: {
        type: String,
        default: 'MMMM Do'
      },

      // Set the default selected date.
      picked: String
    },


    methods: {
      /**
       * Select a date.
       *
       * @author Curtis Blackwell
       * @param  {string} date Date to select.
       * @return {void}
       */
      pick(date) {
        this.selected = date;
      },
    },


    ready() {
      // Set the date based on the `picked` prop.
      this.date     = this.picked ? this.picked : moment();
      this.selected = this.picked;
    },
  };
</script>
