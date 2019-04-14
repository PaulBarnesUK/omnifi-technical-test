<template>
    <div>
      <div class="map w100-mobile"></div>
      <LocationsList :locations="this.locations" @locationSelected="handleLocationSelect"></LocationsList>
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
    handleLocationSelect (location) {
      // Set the center of the map to the selected location
      this.map.setZoom(7)
      this.map.panTo({
        lat: location.latitude,
        lng: location.longitude
      })

      this.google.maps.event.trigger(location.marker, 'click')
    },
    // fetches the locations from the API, returns a promise that resolves with the JSON response
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
    this.google = await googleMapsInit()
    this.locations = await this.fetchLocations()
    const averageGeolocation = calculateAverageGeolocation(this.locationLatLngs)

    // Init our map
    this.map = new this.google.maps.Map(this.$el.querySelector('.map'), {
      center: averageGeolocation,
      zoom: 5
    })

    // Add the markers and popups for each location
    this.locations.forEach(location => {
      const infoWindow = new this.google.maps.InfoWindow({
        content: location.name
      })

      // Create the marker and assign it as a new property for the location
      location.marker = new this.google.maps.Marker(
        {
          position: {
            lat: location.latitude,
            lng: location.longitude
          },
          map: this.map,
          title: location.name
        })

      // Assign click handler to the marker to open the info window
      location.marker.addListener('click', () => infoWindow.open(this.map, location.marker))
    })
  }
}
</script>

<style lang="scss">
    .map {
        flex: 0 0 75%;
        max-width: 75%;
    }

    .w100-mobile {
      height: 100%;
    }

    @media screen and (max-width: 768px) {
      .w100-mobile {
        flex: 0 0 100%;
        max-width: 100%;
        height: 50%;
      }
    }
</style>
