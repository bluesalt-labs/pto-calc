<template>
  <form method="post" id="user-data-form" class="container-flex-row">

    <div class="input-container container-quarter container-gutter">
      <label for="start_date">Start Date</label>
      <input type="text" id="start_date" name="start_date" v-model="userData.start_date" />
    </div>

    <div class="input-container container-quarter container-gutter">
      <label for="start_hours">Initial PTO Hours</label>
      <input type="text" id="start_hours" name="start_hours" v-model="userData.start_hours" />
    </div>

    <div class="input-container container-half container-gutter">
      <label for="debug-output">Debug Output</label>
      <textarea class="v-resize" id="debug-output" name="debug-output" rows="15">{{ formattedUserData }}</textarea>
    </div>

  </form>
</template>

<script>
  import userdata from "../assets/data/userdata.json"

  export default {
    data() {
      return {
        userData: userdata
      }
    },
    computed: {
      formattedUserData: function() {
        return JSON.stringify(this.userData, null, "\t");
      }
    }
  }
</script>
