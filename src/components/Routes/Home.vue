<template>
  <div class="container">
    <h1>PTO Calculator</h1>
    <hr />

    <h2>User Information</h2>
    <UserDataForm
      :userData="userData"
    ></UserDataForm>
    <hr />

    <div class="container-flex-column">
      <div class="container-half container-gutter">
        <h2>PTO Hours</h2>
        <HoursTable
          :userData="userData"
          :dateFormat="config.date_format"
        ></HoursTable>
      </div>

      <div class="container-half container-gutter">
        <h2>Holidays</h2>
        <HolidaysTable
          :holidays="holidays"
          :userData="userData"
          :dateFormat="config.date_format"
        ></HolidaysTable>
      </div>
    </div>

  </div>
</template>

<script>
  import UserDataForm from "../UserDataForm.vue"
  import HoursTable from "../HoursTable.vue"
  import HolidaysTable from "../HolidaysTable.vue"

  import config from "../../assets/data/config.json"
  import userdata from "../../assets/data/userdata.json"
  import holidays from "../../assets/data/holidays.json"

  export default {
    name: 'Home',
    components: { UserDataForm, HoursTable, HolidaysTable },
    data() {
      return {
        config: config,
        holidays: holidays,
        userData: userdata
      }
    }
  }
</script>
