<template>
  <div class="container">
    <h1>PTO Calculator</h1>
    <hr />
    <h2>
      User Information
    </h2>
    <UserDataForm />
  </div>
</template>

<script>
  import UserDataForm from "../UserDataForm.vue"

  export default {
    name: 'Home',
    components: {
      UserDataForm
    }
  }
</script>
