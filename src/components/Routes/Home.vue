<template>
  <div class="container">
    <h1>PTO Calculator</h1>
    <hr />

    <h2>User Information</h2>
    <UserDataForm :userData="userData"></UserDataForm>
    <hr />

    <h2>Hours Table</h2>
    <div class="container-half">
      <HoursTable :userData="userData"></HoursTable>
    </div>

  </div>
</template>

<script>
  import UserDataForm from "../UserDataForm.vue"
  import HoursTable from "../HoursTable.vue"

  import userdata from "../../assets/data/userdata.json"

  export default {
    name: 'Home',
    components: { UserDataForm, HoursTable },
    data() {
      return {
        userData: userdata
      }
    }
  }
</script>
