<template>
    <div>
      <div id="map"></div>
      <LocationsList :locations="this.locations"></LocationsList>
    </div>
</template>

<script>
import LocationsList from '@/components/LocationsList'
import googleMapsInit from '@/utils/googleMaps'
import { calculateAverageGeolocation } from '@/utils/helpers'

export default {
  data () {
    return {
      locations: []
    }
  },

  computed: {
    locationLatLngs () {
      return this.locations.map(location => (
        {
          latitude: location.latitude,
          longitude: location.longitude
        }
      ))
    }
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
  },

  components: {
    LocationsList
  },

  async mounted () {
    const google = await googleMapsInit()
    this.locations = await this.fetchLocations()
    const averageGeolocation = calculateAverageGeolocation(this.locationLatLngs)

    // Init our map
    const map = new google.maps.Map(this.$el.querySelector('#map'), {
      center: averageGeolocation,
      zoom: 5
    })

    // Add the markers and popups for each location
    this.locations.forEach(location => {
      const infoWindow = new google.maps.InfoWindow({
        content: location.name
      })

      // Create the marker and assign it as a new property for the location
      location.marker = new google.maps.Marker(
        {
          position: {
            lat: location.latitude,
            lng: location.longitude
          },
          map,
          title: location.name
        })

      // Assign click handler to the marker to open the info window
      location.marker.addListener('click', () => infoWindow.open(map, location.marker))
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
