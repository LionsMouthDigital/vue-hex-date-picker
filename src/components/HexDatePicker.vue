<template>
  <div class="calendar" v-show="show">
    <header class="calendar-header">
      <div class="controls" v-if="controls">
        <div v-if="controls === true || controls === 'year'">
          <button class="button-prev" @click.prevent="prevYear()">
            <span class="sr-only">Previous year</span>
          </button>
          <span class="active-date">{{ moment(date).format('YYYY') }}</span>
          <button class="button-next" @click.prevent="nextYear()">
            <span class="sr-only">Next year</span>
          </button>
        </div>

        <div v-if="controls === true || controls === 'month'">
          <button class="button-prev" @click.prevent="prevMonth()">
            <span class="sr-only">Previous month</span>
          </button>
          <span class="active-date">{{ moment(date).format('MMM') }}</span>
          <button class="button-next" @click.prevent="nextMonth()">
            <span class="sr-only">Next month</span>
          </button>
        </div>
      </div>

      <div class="days-of-week">
        <div v-for="day in ['S', 'M', 'T', 'W', 'Th', 'F', 'Sa']">{{ day }}</div>
      </div>
    </header>

    <div class="week" v-for="week in weeks">
      <div
        @click = "pick(day)"
        :class = "getDateClasses(day)"
        v-for  = "day in week"
      >
        {{ moment(day, format).format('DD') }}
      </div>
    </div>
  </div>
</template>




<script>
  import moment      from 'moment';
  import HexCalendar from 'vue-hex-calendar';

  export default {
    name: 'HexDatePicker',

    extends: HexCalendar,


    props: {
      /**
       * Specify controls to show.
       *
       * Acceptable values include:
       *  - `true` for all,
       *  - `false` for none,
       *  - `month` for only the month, or
       *  - `year` for only the year (for some reason...).
       *
       * @type {Object}
       */
      controls: {
        type:    [Boolean, String],
        default: true,
      },

      /**
       * Dates to disable.
       *
       * Accepts a pipe-separated list of dates in `YYYY-MM-DD` format.
       *
       * @type {String}
       */
      disableDates:  String,
      /**
       * Days to disable.
       *
       * Accepts a pipe-separated list of days of the week.
       *
       * @type {String}
       */
      disableDays: String,
      /**
       * Disable past dates.
       *
       * @type {Boolean}
       */
      disablePast: Boolean,
      /**
       * Disable today's date.
       *
       * @type {Boolean}
       */
      disableToday: Boolean,
      /**
       * Disable future dates.
       *
       * @type {Boolean}
       */
      disableFuture: Boolean,

      /**
       * Set the user-friendly date format.
       *
       * See [Moment.js docs](http://momentjs.com/docs/#year-month-and-day-tokens).
       *
       * @type {String}
       */
      displayFormat: {
        type:    String,
        default: 'MMMM Do',
      },

      /**
       * Value of `ref` for the `input` to push the submittable value to.
       *
       * @type {String}
       */
      input:        String,
      /**
       * Value of `ref` for the `input` to push the display value to.
       *
       * @type {String}
       */
      inputDisplay: String,

      /**
       * Picked date on load.
       *
       * @type {String}
       */
      picked: {
        type:    String,
        default: '',
      },

      /**
       * Pass `true` to show the calendar on load.
       *
       * @type {Object}
       */
      visible: {
        type:    Boolean,
        default: false,
      },
    },


    data() {
      return {
        /**
         * Base date to build the calendar on. `picked` prop if specified, falls back to today.
         * The rest of the current month displays.
         *
         * @type {String}
         */
        date: '',

        /**
         * Concatenation of `this.disableDates` and `this.disableDays`.
         *
         * @type {Array}
         */
        disabled: [],

        /**
         * Moment JS formatting for precise dates.
         *
         * @type {String}
         */
        formatPrecise: 'YYYY-MM-DD HH:mm',

        /**
         * Selected date.
         *
         * Use `this.pick()` to set.
         *
         * @type {String}
         */
        selected: '',

        /**
         * Today's date in `YYYY-MM-DD` format.
         *
         * @type {String}
         */
        today: moment().format('YYYY-MM-DD'),

        /**
         * Show the calendar?
         *
         * @type {Boolean}
         */
        show: false,
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
        return this.selected ? moment(this.selected).format(this.displayFormat) : '';
      },
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
        let classes = 'day';
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
        let datePrecise   = moment(date + ' 00:00', this.formatPrecise);
        let todayPrecise  = moment(moment().format(this.format) + ' 00:00', this.formatPrecise);
        let diffFromToday = datePrecise.diff(todayPrecise, 'days');

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
        this.updateInputValues();

        // If this date-picker wasn't visible on load, hide it after giving the user a split-second
        // to see their picked date selected.
        if (!this.visible) {
          setTimeout(() => {
            this.show = false;
          }, 100);
        }
      },

      /**
       * Update the values of inputs.
       *
       * @author Curtis Blackwell
       * @return {void}
       */
      updateInputValues() {
        // Value to submit to form.
        if (typeof this.$parent.$refs[this.input] !== 'undefined') {
          this.$parent.$refs[this.input].value = this.selected;
        }

        // Value to display to user.
        if (typeof this.$parent.$refs[this.inputDisplay] !== 'undefined') {
          this.$parent.$refs[this.inputDisplay].value = this.displayDate;
        }
      },
    },


    created() {
      // Set the date based on the `picked` prop.
      this.date     = this.picked ? this.picked : this.today;
      this.selected = this.picked;

      // Show if `visible` prop is `true`.
      this.show = this.visible;

      // Create an array of disabled dates based on the `disable` prop.
      let disabledDays  = typeof this.disableDays  === 'string' ? this.disableDays.split('|')  : [];
      let disabledDates = typeof this.disableDates === 'string' ? this.disableDates.split('|') : [];

      this.disabled = disabledDays.concat(disabledDates);
    },


    mounted() {
      this.updateInputValues();
    },
  };
</script>
