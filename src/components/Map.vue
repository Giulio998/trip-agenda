<template>
  <div id="map" style="width: 100%; height: 300px"></div>
</template>
<script>
import tt from '@tomtom-international/web-sdk-maps'
import axios from 'axios'

export default {
  data() {
    return {}
  },
  watch: {
    locations: {
      handler() {
        if (this.locations.length > 0) this.markerMap()
      },
      deep: true
    }
  },
  props: {
    locations: Array
  },
  methods: {
    genMap() {
      var map = tt.map({
        name: 'map',
        key: 'qicXsxpR2GuZBNAJUJA0XBi1PHp4OzgD',
        container: 'map',
        
      })
    },
    markerMap() {
      var map = tt.map({
        name: 'map',
        key: 'qicXsxpR2GuZBNAJUJA0XBi1PHp4OzgD',
        container: 'map',
        zoom: 14,
        center: this.locations[0]
        
      })
      this.locations.forEach((element) => {
        console.log(element)
        var marker = new tt.Marker().setLngLat(element).addTo(map)
        
      })
    }
  },

  mounted() {
    this.genMap()
  }
}
</script>

<style lang="scss" scoped></style>
