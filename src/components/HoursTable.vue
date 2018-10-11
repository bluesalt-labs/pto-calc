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
        <td>{{ rowData.date }}</td>
        <td>{{ rowData.hours }}</td>
        <td>{{ rowData.hours / 8 }}</td>
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
        required: false,
        default: "MM-DD-YYYY"
      }
    },
    computed: {
      dateRows: function() {
        var output = [];
        var currentHours = this.userData.start_hours;
        var hoursPerDate = this.userData.hours_per_period;

        var hoursUsedDates = this.getHoursUsed();
        var payDates = this.getDateValues();

        var todayDate = moment();

        for(var i in payDates) {
          // todo: should subtract hours?
          var payDate = moment(payDates[i]);


          output.push({
            "date": payDate.format(this.dateFormat),
            "hours": currentHours,
            "is_future": (payDate > todayDate),
            "is_add": true
           });

          currentHours += hoursPerDate;
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
              output.push( rowDate.format() );
            }
          }
        }

        return output;
      },
      getHoursUsed: function() {
        var output = [];

        for(var i in this.userData.hours_used) {
          var hours = 0;

          try {
            hours = parseInt(this.userData.hours_used[i].hours)
          } catch(e) {}

          if(hours > 0) {
            output.push({
              "date": moment(this.userData.hours_used[i].date),
              "hours": hours
            })
          }
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
