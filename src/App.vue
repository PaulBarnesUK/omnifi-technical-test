<template>
  <div id="app">
    <Map></Map>
  </div>
</template>

<script>
import Map from '@/components/Map'

export default {
  name: 'app',
  components: {
    Map
  }
}
</script>

<style lang="scss">
  body, html {
    height: 100%;
    margin: 0;
    overflow: hidden;
    padding: 0;
  }

  #app {
    display: flex;
    height: 100%;
  }
</style>
