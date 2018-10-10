<template>
  <form method="post" id="user-data-form">
    <textarea>
      {{ JSON.stringify(userData) }}
    </textarea>
  </form>
</template>

<script>
  import userdata from "../assets/data/userdata.json"

  export default {
    data() {
      return {
        userData: userdata
      }
    }
  }
</script>
