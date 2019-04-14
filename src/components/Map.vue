<template>
    <div id="map"></div>
</template>

<script>
import googleMapsInit from '@/utils/googleMaps'

export default {
  async mounted () {
    const google = await googleMapsInit()

    // Init our map
    // eslint-disable-next-line
    const map = new google.maps.Map(this.$el, {
      center: {
        lat: 0,
        lng: 0
      },
      zoom: 6
    })
  }
}
</script>

<style lang="scss">
    #map {
        flex: 0 0 75%;
        max-width: 75%;
    }
</style>
