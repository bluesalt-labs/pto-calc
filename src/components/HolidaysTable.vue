<template>
  <table>
    <thead>
      <tr>
        <th></th>
        <th>Date</th>
        <th>Name</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(rowData, index) in holidayRows">
        <td>{{ index + 1 }}</td>
        <td>{{ rowData.date.format(dateFormat) }}</td>
        <td>{{ rowData.title }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
  import moment from "moment"

  export default {
    props: {
      holidays: {
        type: Object,
        required: true
      },
      dateFormat: {
        type: String,
        required: true
      },
      userData: {
        type: Object,
        required: true
      }
    },
    computed: {
      dateRange: function() {
        return {
          today:  moment(),
          start:  moment().subtract(1, "year"), // temp
          end:    moment().add(1, "year"),      // temp
        };
      },
      holidayRows: function() {
        var output = [];

        var sortOutputCallback = function(a, b) {
          if( a.date.isAfter(b.date)  ) { return 1; }
          if( a.date.isBefore(b.date) ) { return -1; }
          return 0;
        };

        if(this.dateRange.start < this.dateRange.end) {
          // Iterate over each holiday
          for(var i in this.holidays) {
            var loopDateRange = {
              start:  this.dateRange.start,
              end:    null
            };

            // Iterate over each year in the date range
            for(
              var year = this.dateRange.start.year();
              year <= this.dateRange.end.year();
              year++
            ) {
              // Figure out what the loop's end date should be
              loopDateRange.end = moment().set({ "year": year, "month": 11, "date": 31 });

              // Don't allow the loop end date to be outside dateRange
              if(loopDateRange.end > this.dateRange.end) {
                loopDateRange.end = this.dateRange.end;
              }


              // Get the holiday instance in the loop's current year
              var holiday = {
                date:   this.getHolidayDate(this.holidays[i], year),
                title:  this.holidays[i]["title"]
              };

              if(
                holiday.date &&
                holiday.date >= loopDateRange.start &&
                holiday.date <= loopDateRange.end
              ) { output.push(holiday); }

              // Set the loop's start date to a day after its end date
              loopDateRange.start = moment( loopDateRange.end ).add(1, "day");
            }
          }
        }

        // Add floating holidays
        var floatHolidays = this.getFloatingHolidays();

        for(var i in floatHolidays) {
          // If the floating holiday is in the date range, add it to the output
          if(
            floatHolidays[i] &&
            floatHolidays[i] >= this.dateRange.start &&
            floatHolidays[i] <= this.dateRange.end
          ) {
            output.push({
              date:   floatHolidays[i],
              title:  "Floating Holiday"
            });
          }
        }

        return output.sort(sortOutputCallback);
      }
    },
    methods: {
      getHolidayDate: function(holidayData, year) {
        var holidayDate = moment();

        // Set the year
        holidayDate.year(year);

        // Set the month
        if(holidayData["month"]) {
          holidayDate.month( holidayData["month"] );

          // Set the day
          if(holidayData["date"]) {
            holidayDate.date( holidayData["date"] );
          }
          else if(holidayData["rule"]) {
            holidayDate.date( this.getNthDayInMonth(year, holidayData["month"], holidayData["rule"]) );
          }
          else { holidayDate = null; }
        } else { holidayDate = null; }

        return holidayDate;
      },
      getNthDayInMonth: function(year, month, rule) {
        var day = moment().set({ 'year': year, 'month': month, 'day': rule[0] });

        if(rule[1] > 1) {
          day.add(7 * rule[1], 'days');
        }
        else if(rule[1] === -1) {
          var prevDate = day.date();

          for(var i = 0; i < 8; i++){
            day.add(7, 'days');

            if(day.date() < prevDate) {
              day.subtract(7, 'days');
              break;
            }
          }
        }

        return day.date();
      },
      getFloatingHolidays: function() {
        var output = [];
        var floatingHolidays = this.userData["floating_holidays"];

        if(typeof floatingHolidays === 'object') {
          for(var i in floatingHolidays) {
            try {
              output.push( moment(floatingHolidays[i]) );
            } catch (e) {
              console.warn("Could not add floating holiday value: '" + floatingHolidays[i] + "'");
            }
          }
        }

        return output;

      }
    }
  }
</script>

<style lang="scss">
  @import "./src/assets/scss/base.scss";



</style>
