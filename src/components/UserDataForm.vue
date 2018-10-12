<template>
  <form method="post" id="user-data-form" class="container-flex-row">

    <div class="container-third container-gutter">
      <h3>User Information</h3>
      <div class="input-container">
        <label for="hours_per_period">Hours Accrued<br />(per pay period)</label>
        <input type="number" min="0" max="80" step="1" id="hours_per_period" name="hours_per_period" v-model="userData.hours_per_period" />
      </div>

      <div class="input-container">
        <label for="start_date">Start Date</label>
        <input type="text" id="start_date" name="start_date" v-model="userData.start_date" />
      </div>

      <div class="input-container">
        <label for="pay_period_days">Days Pay Period Ends<br />(in a given month)</label>
        <input type="text" id="pay_period_days" name="pay_period_days" v-model="userData.pay_period_days" />
      </div>

      <div class="input-container">
        <label for="start_hours">Initial PTO Hours</label>
        <input type="text" id="start_hours" name="start_hours" v-model="userData.start_hours" />
      </div>

    </div>

    <div class="container-third container-gutter">
      <h3>Hours Used</h3>
      <div  v-for="(entry, index) in userData.hours_used">

        <div class="input-container container-half">
          <label :for="'hours_used_date-' + index">Date</label>
          <input type="text" :id="'hours_used_date-' + index" :name="'hours_used_date-' + index" v-model="entry.date" />
        </div>

        <div class="input-container container-half">
          <label :for="'hours_used_hours-' + index">Hours</label>
          <input type="text" :id="'hours_used_hours-' + index" :name="'hours_used_hours-' + index" v-model="entry.hours" />
        </div>

      </div>
    </div>

    <div class="input-container container-third container-gutter">
      <label for="debug-output"><strong>Debug Output</strong></label>
      <textarea class="v-resize" id="debug-output" name="debug-output" rows="15">{{ JSON.stringify(this.userData, null, "\t") }}</textarea>
    </div>

  </form>
</template>

<script>
  export default {
    props: ['userData']
  }
</script>
