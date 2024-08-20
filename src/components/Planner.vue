<template>
  <!-- ACCORDION -->
  <div class="accordion accordion-flush" id="accordionFlushExample">
    <div v-for="(n, index) in days" class="accordion-item">
      <h2 class="accordion-header">
        <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse"
          :data-bs-target="`#flush-collapseOne-` + n" aria-expanded="false" :aria-controls="`flush-collapseOne-` + n">
          GIORNO {{ n }}
        </button>
      </h2>
      <div :id="`flush-collapseOne-` + n" class="accordion-collapse collapse" data-bs-parent="#accordionFlushExample">
        <div class="accordion-body">
          <template v-if="myTrip.activities">
            <div class="row" v-for="activity in myTrip.activities[n - 1]">
              <div class="col-4">{{ activity.name }}</div>
              <div class="col-4">{{ activity.time }}</div>
              <div class="col-4">{{ activity.note }}</div>
            </div>
          </template>

          <button type="button" @click="openModal(n)">Aggiungi</button>
        </div>
      </div>
      <!-- MODAL -->
      <div class="backgroundModal" :id="'activity-modal-' + n">
        <div class="customModal">
          <div class="close" @click="closeModal(n)">X</div>
          <div>
            <h3>GIORNO {{ n }}</h3>
            <div class="row justify-space-around">
              <div class="col-6">
                <div class="mb-2">
                  <label for="name" class="form-label">Nome</label>
                  <input type="text" class="form-control" placeholder="Nome" v-model="activity.name" />
                </div>

                <div class="mb-2">
                  <label for="" class="form-label">Ora</label>
                  <input type="time" class="form-control" v-model="activity.time" />
                </div>

                <div class="mb-2">
                  <label for="" class="form-label">Note</label>
                  <textarea class="form-control" v-model="activity.note"></textarea>
                </div>

                <button :disabled="emptyFields() ? true : false" @click="pushActivity(n)">Aggiungi</button>
              </div>
              <div class="col-6">
                <label for="" class="form-label">Luogo</label>
                <div id="searchbox"></div>
                
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import { services } from '@tomtom-international/web-sdk-services'
import SearchBox from '@tomtom-international/web-sdk-plugin-searchbox'
import axios from 'axios'

export default {
  data() {
    return {
      activity: {
        name: '',
        time: '',
        note: '',
        locationCoordinates: []
      }
    }
  },
  emits: ['fetchTrips'],
  props: { days: Number, myTrip: Object },
  methods: {
    emptyFields(){
      if (this.activity.locationCoordinates.length === 0 || this.activity.name === '') {
       
        return true
      } else {
        
        return false
      }
    },
    test() {
      let searchTerm = document.querySelector('.tt-search-box-input').value
      console.log('Cerco questo: ' + searchTerm)
    },
    searchBar() {
      var options = {
        searchOptions: {
          key: 'qicXsxpR2GuZBNAJUJA0XBi1PHp4OzgD',
          language: 'it-IT',
          limit: 5
        },
        autocompleteOptions: {
          key: 'qicXsxpR2GuZBNAJUJA0XBi1PHp4OzgD',
          language: 'it-IT'
        }
      }
      var ttSearchBox = new SearchBox(services, options)
      var searchBoxHTML = ttSearchBox.getSearchBoxHTML()
      const searchBox = document.getElementById('searchbox')
      searchBox?.append(searchBoxHTML)
      ttSearchBox.on('tomtom.searchbox.resultselected', this.handleResultSelection)
    },
    handleResultSelection(event) {
      var result = event.data.result
      let lat = result.position.lat
      let lon = result.position.lng

      this.activity.locationCoordinates = [lon, lat]
      console.log("activity name",this.activity.name)
    },
    pushActivity(day) {
      let index = day - 1
      this.myTrip.activities[index].push(this.activity)
      let trips = localStorage.getItem('trips')
      const myTrips = JSON.parse(trips)
      let currentUrl = window.location.href
      let tripId = currentUrl.substring(currentUrl.lastIndexOf('/') + 1)
      myTrips[tripId] = this.myTrip
      localStorage.setItem('trips', JSON.stringify(myTrips))
      // this.search()
      this.closeModal(day)
      this.$emit('fetchTrips')
    },
    openModal(day) {
      let modal = document.getElementById('activity-modal-' + day)
      modal.style.display = 'block'
      this.searchBar()
    },
    closeModal(day) {
      let modal = document.getElementById('activity-modal-' + day)
      modal.style.display = 'none'
    }
    // async search() {
    //   let searchTerm = document.querySelector('.tt-search-box-input').value
    //   await axios
    //     .get(
    //       `https://api.tomtom.com/search/2/autocomplete/${searchTerm}.json?key=qicXsxpR2GuZBNAJUJA0XBi1PHp4OzgD&limit=10&language=it-IT`
    //     )
    //     .then((response) => {
    //       console.log(response)

    //       let lat = response.data.results[0].position.lat
    //       let lon = response.data.results[0].position.lon
    //       this.activity.locationCoordinates = [lon, lat]
    //     })
    //     .catch((error) => {
    //       console.error(error)
    //     })
    // }
  },

  mounted() {
    // this.fetchTrips()
    // this.searchBar()
  }
}
</script>

<style lang="scss" scoped>
.customModal {

  background-color: white;
  z-index: 5;
  width: 800px;
  overflow: hidden;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 10px 5px 0 rgba(black, 0.2);



  .close {
    position: absolute;
    right: 30px;
    top: 20px;

    &:hover {
      transform: scale(1.1);
    }
  }

  #searchbox>* {
    margin-top: 0 !important;
  }
}

.backgroundModal {

  display: none;
  background: rgba(0, 0, 0, 0.5);
  z-index: 5;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);


}
</style>
