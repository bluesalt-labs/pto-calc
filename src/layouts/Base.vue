<template>
  <div class="main-container">
    <slot />
  </div>
</template>

<script>
  export default {
    name: 'Base'
  }
</script>

<style lang="scss">
  /* colors */
  $color-default: #8FA2AA;

  $color-stormy: #494E6B;
  $color-cloud: #98878F;
  $color-sunset: #985E6D;
  $color-evening: #192231;

  $color-navy: #14141E;
  $color-gunmetal: #233237;
  $color-rusty: #984B43;
  $color-warm: #EAC67A;

  $color-primary: $color-evening;
  $color-secondary: $color-stormy;
  $color-text: $color-default;
  $color-warning: $color-warm;
  $color-danger: $color-rusty;

  /* end colors */

  /* Resets */
  html, body, #app {
    background: transparent;
    position: absolute;
    height: 100vh;
    width: 100vw;
  }

  body {
    line-height: 1;
  }

  html, body, #app, .main-container {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    margin: 0;
    padding: 0;
    border: 0;
    top: 0;
    left: 0;
    font-size: 100%;
    box-sizing: border-box;
    vertical-align: baseline;
    color: $color-text;
  }
  /* end resets */

  #app {
    background: $color-primary
  }

  .main-container {

  }
</style>
