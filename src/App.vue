<template>
  <div id="app">
    <component :is="layout">
      <router-view/>
    </component>
  </div>
</template>

<script>
const default_layout = "base"

export default {
  computed: {
    layout() {
      return (this.$route.meta.layout || default_layout) + '-layout'
    }
  }
}
</script>
