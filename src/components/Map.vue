<template>
    <div id="map"></div>
</template>

<script>
import googleMapsInit from '@/utils/googleMaps'

export default {
  data () {
    return {
      locations: []
    }
  },

  async mounted () {
    const google = await googleMapsInit()

    this.locations = await this.fetchLocations()

    // Init our map
    const map = new google.maps.Map(this.$el, {
      center: {
        lat: 0,
        lng: 0
      },
      zoom: 6
    })

    // Create the markers and add them to our markers array
    this.locations.forEach(location => {
      location.marker = new google.maps.Marker(
        {
          position: {
            lat: location.latitude,
            lng: location.longitude
          },
          map,
          title: location.name
        })
    })
  },

  methods: {
    fetchLocations () {
      let resolvePromise
      let rejectPromise

      fetch('https://s3-eu-west-1.amazonaws.com/omnifi/techtests/locations.json', {
        method: 'get'
      }).then(response => {
        resolvePromise(response.json())
      }).catch(error => {
        rejectPromise(error)
      })

      return new Promise((resolve, reject) => {
        resolvePromise = resolve
        rejectPromise = reject
      })
    }
  }
}
</script>

<style lang="scss">
    #map {
        flex: 0 0 75%;
        max-width: 75%;
    }
</style>
