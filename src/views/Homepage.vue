<template>
  <div class="container-md">
    <template v-if="myTrips.length > 0">
      <h1 class="text-warning text-center display-2">Vacanze programmate</h1>
      <div class="row">
        <div class="d-flex  align-items-center">
          <div class="tripCard p-2" v-for="(trip, index) in myTrips">
            <Card :trip="trip" :index="index" />
            <!-- <RouterLink :to="{ name: 'trip.show', params: { index: index } }">
        <img :src="trip.image" alt="Immagine vacanza" width="300px" />
        <h2>{{ trip.name }}</h2>
        <p>{{ trip.start_date }} - {{ trip.end_date }}</p>
      </RouterLink> -->
          </div>
        </div>
      </div>


    </template>
    <template v-else>
      <div class="d-flex  align-items-center">
        <h1>Nessuna vacanza programmata</h1>
      </div>


    </template>



    <RouterLink :to="{ name: 'trip.create' }">
      <div>
        <button class="btn btn-warning" id="addTrip">
          <i class="bi bi-calendar-plus"></i>
        </button>
      </div>
    </RouterLink>
  </div>
</template>

<script>
import Card from '../components/Card.vue'
export default {
  data() {
    return {
      myTrips: []
    }
  },
  components: {
    Card
  },

  methods: {
    fetchTrips() {
      let trips = localStorage.getItem('trips')

      if (trips) {
        this.myTrips = JSON.parse(trips)
      }
    }
  },
  mounted() {
    this.fetchTrips()
  }
}
</script>

<style lang="scss">
@use '../style/partials/mixins' as *;
@use '../style/partials/variables' as *;

#addTrip {
  width: 5vw;
  height: 5vw;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;

  i {
    font-size: 2rem;
    font-weight: 900;
    color: white;
    text-align: center;
  }
}
</style>
