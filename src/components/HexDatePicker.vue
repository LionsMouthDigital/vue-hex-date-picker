<template>
  <div class="calendar">
    <table>
      <thead>
        <tr>
          <th colspan="7">
            <div class="controls">
              <div>
                <button @click="prevMonth()" class="button-prev"></button>
                <span class="active-date">{{ moment(date).format('MMM') }}</span>
                <button @click="nextMonth()" class="button-next"></button>
              </div>

              <div>
                <button @click="prevYear()" class="button-prev"></button>
                <span class="active-date">{{ moment(date).format('YYYY') }}</span>
                <button @click="nextYear()" class="button-next"></button>
              </div>
            </div>
          </th>
        </tr>

        <tr class="days-of-week">
          <th v-for="day in daysOfWeek">{{ day }}</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="week in weeks">
          <td
            v-for  = "day in week"
            @click = "pick(day)"
            class  = "{{ getDateClasses(day) }}"
          >
            {{ moment(day, format).format('DD') }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import moment      from 'moment';
  import HexCalendar from 'vue-hex-calendar';

  export default {
    extends: HexCalendar,

    data() {
      return {
        // Base date to build the calendar on. If `picked` prop is unspecified, it will default to
        // today, and the rest of the current month will be shown.
        date:          '',
        disabled:      '',
        formatPrecise: 'YYYY-MM-DD HH:mm',
        // Selected date.
        selected:      '',
        today:         moment().format('YYYY-MM-DD'),
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
      // Prevent dates from getting picked.
      // Accepts a pipe-separated list of dates in `YYYY-MM-DD` format.
      disableDates:  String,
      // Accepts a pipe-separated list of days of the week.
      disableDays:   String,
      disablePast:   Boolean,
      disableToday:  Boolean,
      disableFuture: Boolean,

      // Set the user-friendly date format.
      // See [Moment.js docs](http://momentjs.com/docs/#year-month-and-day-tokens).
      displayForamt: {
        type: String,
        default: 'MMMM Do'
      },

      // Set the default selected date.
      picked: String,
    },


    methods: {
      /**
       * Get appropriate classes for a given date.
       *
       * @author Curtis Blackwell
       * @param  {string} date Date to get classes for.
       * @return {string}
       */
      getDateClasses(date) {
        var classes = '';
        // For today.
        if (this.today === date) {
          classes += ' today';
        }
        // For currently picked date.
        if (date === this.selected) {
          classes += ' picked';
        }
        // For disabled dates.
        if (this.isDisabled(date)) {
          classes += ' disabled';
        }
        // For days in inactive months.
        if ((this.date).split('-')[1] !== date.split('-')[1]) {
          classes += ' in-inactive-month';
        }

        return classes;
      },

      /**
       * Determine whether a given date is disabled.
       *
       * @author Curtis Blackwell
       * @param  {string}  date Date to check.
       * @return {Boolean}
       */
      isDisabled(date) {
        var datePrecise   = moment(date + ' 00:00', this.formatPrecise);
        var todayPrecise  = moment(moment().format(this.format) + ' 00:00', this.formatPrecise);
        var diffFromToday = datePrecise.diff(todayPrecise, 'days');

        return this.disabled.indexOf(date) > -1                        ||
               this.disabled.indexOf(moment(date).format('dddd')) > -1 ||
               this.disablePast   && diffFromToday < 0                 ||
               this.disableToday  && date === this.today               ||
               this.disableFuture && diffFromToday > 0;
      },

      /**
       * Select a date.
       *
       * @author Curtis Blackwell
       * @param  {string} date Date to select.
       * @return {void}
       */
      pick(date) {
        // Ensure the date isn't disabled and select it.
        this.selected = !this.isDisabled(date) ? date : this.selected;
      },
    },


    ready() {
      // Set the date based on the `picked` prop.
      this.date     = this.picked ? this.picked : moment().format(this.format);
      this.selected = this.picked;

      // Create an array of disabled dates based on the `disable` prop.
      var disabledDays  = typeof this.disableDays  === 'string' ? this.disableDays.split('|')  : [];
      var disabledDates = typeof this.disableDates === 'string' ? this.disableDates.split('|') : [];

      this.disabled = disabledDays.concat(disabledDates);
    },
  };
</script>
