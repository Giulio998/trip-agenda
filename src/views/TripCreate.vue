<template>
  <div class="container-md">
    <div><h1>Crea la tua vacanza</h1></div>
    <div>
      <label for="name">Nome</label>
      <input
        type="text"
        placeholder="Nome"
        id="name"
        name="name"
        v-model="this.newTrip.name"
        required
      />
    </div>
    <div>
      <label for="start_date">Data di inizio</label>
      <input
        type="date"
        id="start_date"
        name="start_date"
        v-model="newTrip.start_date"
        required
        @input="valiDate()"
      />
    </div>
    <div>
      <label for="end_date">Data di fine</label>
      <input
        type="date"
        id="end_date"
        name="end_date"
        v-model="newTrip.end_date"
        required
        @input="valiDate()"
      />
    </div>
    <div>
      <label for="thumbnail">Immagine di copertina</label>
      <input
        type="file"
        id="thumbnail"
        name="thumbnail"
        accept="image/png, image/jpeg"
        @change="uploadImage()"
      />
      <img :src="newTrip.image" width="300px" />
    </div>
    <router-link :to="{ name: 'home' }" class="router-link"><button class="btn btn-warning" value="Invia" @click="addTrip()">Invia</button></router-link>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTrip: {
        name: '',
        start_date: null,
        end_date: null,
        image: null,
        days: null,
        activities: []
      },
      validated: null,
      myTrips: []
    }
  },

  methods: {
    valiDate() {
      if (new Date(this.newTrip.start_date) < new Date(this.newTrip.end_date)) {
        this.validated = true
        this.calculateDays()
      } else {
        this.validated = false
      }
    },
    generateDays() {
      for (let i = 0; i < this.newTrip.days; i++) {
        const dayActivities = []
        this.newTrip.activities.push(dayActivities)
      }
    },
    addTrip() {
      if (this.validated == false) {
        alert('La data di inizio non può essere successiva alla data di fine')
        return
      }
      this.generateDays()
      this.myTrips.push(this.newTrip)
      localStorage.setItem('trips', JSON.stringify(this.myTrips))
    },
    fetchTrips() {
      let trips = localStorage.getItem('trips')

      if (trips) {
        this.myTrips = JSON.parse(trips)
      }
    },
    calculateDays() {
      const start = new Date(this.newTrip.start_date)
      const end = new Date(this.newTrip.end_date)
      const timeDifference = end.getTime() - start.getTime()
      this.newTrip.days = Math.floor(timeDifference / (1000 * 3600 * 24))
    },
    uploadImage() {
      const file = document.querySelector('input[type=file]').files[0]
      const reader = new FileReader()

      reader.onloadend = () => {
        const rawImg = reader.result
        this.newTrip.image = rawImg // Base64-encoded image
        console.log(rawImg) // Base64-encoded image
      }

      reader.readAsDataURL(file)
    }
  },

  mounted() {
    this.fetchTrips()
  }
}
</script>

<style lang="scss" scoped></style>
