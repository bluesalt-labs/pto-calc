<template>
  <div class="main-container">
    <Header id="header" />
    <div id="content">
      <slot />
    </div>
    <Footer id="footer" />
  </div>
</template>

<script>
  import Header from "./Header.vue"
  import Footer from "./Footer.vue"

  export default {
    name: 'Base',
    components: {
      Header,
      Footer
    }
  }
</script>

<style lang="scss">
  @import "./src/assets/scss/base.scss";
</style>
