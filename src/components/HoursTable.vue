<template>
  <table>
    <thead>
      <tr>
        <th></th>
        <th>Date</th>
        <th>PTO Hours</th>
        <th>PTO Days</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="(rowData, index) in dateRows"
        :class="( rowData.is_add ? (rowData.is_future ? 'date-row-future' : 'date-row-past') : 'date-row-subtract')"
      >
        <td>{{ index + 1}}</td>
        <td>{{ rowData.date.format(dateFormat) }}</td>
        <td>{{ rowData.hours }}</td>
        <td>{{ (rowData.hours / 8).toFixed(2) }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
  import moment from "moment"

  export default {
    props: {
      userData: {
        type: Object,
        required: true
      },
      dateFormat: {
        type: String,
        required: true
      }
    },
    computed: {
      dateRows: function() {
        var output = [];

        var usedDates = this.getHoursUsed();
        var payDates  = this.getDateValues();
        var todayDate = moment();

        var sortOutputCallback = function(a, b) {
          if( a.date.isAfter(b.date)  ) { return 1; }
          if( a.date.isBefore(b.date) ) { return -1; }
          return 0;
        };

        // Add PTO dates
        for(var i in payDates) {
          output.push({
            "date":       payDates[i],
            "hours":      0,
            "is_future":  (payDates[i] > todayDate),
            "is_add":     true
          });
        }

        // Add PTO hours taken
        for(var i in usedDates) {
          output.push({
            "date":       usedDates[i].date,
            "hours":      usedDates[i].hours,
            "is_future":  (usedDates[i] > todayDate),
            "is_add":     false
          });
        }

        if(output.length > 0) {
          // Sort the output array before continuing
          output.sort(sortOutputCallback);

          var hoursPerDate = parseInt(this.userData.hours_per_period);
          var prevHours = this.userData.start_hours;

          for(var i in output) {
            if(output[i].is_add) {
              output[i].hours = prevHours;
              prevHours += hoursPerDate;
            } else {
              prevHours += output[i].hours;
            }
          }
        }

        return output;
      }
    },
    methods: {
      getDateValues: function() {
        var output = [];

        var startDate = moment(this.userData.start_date);
        var endDate   = moment().add(2, 'months');

        for(var rowDate = moment(startDate); rowDate <= endDate; rowDate.add(1, 'days')) {
          for(var i in this.userData.pay_period_days) {
            if(rowDate.date() === parseInt(this.userData.pay_period_days[i])) {
              output.push( moment(rowDate) );
            }
          }
        }

        return output;
      },
      getHoursUsed: function() {
        var output = [];

        for(var i in this.userData.hours_used) {
          output.push({
            "date": moment(this.userData.hours_used[i].date),
            "hours": -Math.abs(parseInt(this.userData.hours_used[i].hours))
          })
        }

        return output;
      }
    }
  }
</script>

<style lang="scss">
  @import "./src/assets/scss/base.scss";

  .date-row-past     { background-color: rgba($color-success, 0.9); color: $color-navy; }
  .date-row-future   { background-color: rgba($color-cloud, 0.9); color: $color-stormy; }
  .date-row-subtract { background-color: rgba($color-danger, 0.8); }
</style>
