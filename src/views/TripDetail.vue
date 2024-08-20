<template>
  <div class="container">
    <div class="row p-2">
      <div class="col-6 d-flex flex-column">
        <div class="tripCard d-flex mb-3">

          <div class="cardBody">

            <span>Nome viaggio : </span>
            <h1>{{ myTrip.name }}</h1>
            <h5>Data di inizio : {{ myTrip.start_date }}</h5>
            <h5> Data di fine : {{ myTrip.end_date }}</h5>

          </div>
          <div v-if="myTrip.image" class="cardHead p-1">

            <img :src="myTrip.image" alt="Immagine vacanza" width="300px" max-heigth="150px" />
            
          </div>

        </div>
        <div id="mapContainer">
          <Map :key="locations" :locations="locations"></Map>
        </div>
      </div>
      <div class="col-6 d-flex flex-column">
        <Planner :days="myTrip.days" :myTrip="myTrip" @fetchTrips="this.fetchTrips()"></Planner>
      </div>

    </div>




  </div>
</template>

<script>
import Planner from '../components/Planner.vue'
import Map from '../components/Map.vue'

import tt from '@tomtom-international/web-sdk-maps'
import axios from 'axios'

export default {
  data() {
    return {
      myTrip: [],
      locations: []
    }
  },

  methods: {
    fetchTrips() {
      let trips = localStorage.getItem('trips')

      if (trips) {
        const myTrips = JSON.parse(trips)
        let currentUrl = window.location.href
        let tripId = currentUrl.substring(currentUrl.lastIndexOf('/') + 1)
        console.log(tripId)
        this.myTrip = myTrips[tripId]
        this.retrieveMarkers(myTrips[tripId].activities)
      }
    },
    retrieveMarkers(activities) {
      activities.forEach((subArray) => {
        subArray.forEach((item) => {
          if (item.locationCoordinates) {
            this.locations.push(item.locationCoordinates)
          }
        })
      })
    }
  },
  components: { Planner, Map },

  mounted() {
    this.fetchTrips()
  }
}
</script>

<style lang="scss" scoped>
.tripCard {}
</style>

<!-- 
function addMarker(map) { 
  const tt = window.tt; 
  var location = [-121.91595, 37.36729]; 
  var popupOffset = 25; 

  var marker = new tt.Marker().setLngLat(location).addTo(map); 
  var popup = new tt.Popup({ offset: popupOffset }).setHTML("Your address!"); 
          marker.setPopup(popup).togglePopup(); 
}  -->
