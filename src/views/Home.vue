<template>
  <div class="home">
    <h1>Toronto Shelter Occupancy</h1>
    <div class="loader" v-if="loading">loading</div>
    <section class="hero-content" v-if="!loading">
      <h2>{{this.months[`${new Date().getMonth()}`]}} {{new Date().getDate() - 1}} Statistics:</h2>
      <p>{{this.occupiedBeds}} / {{this.totalBeds}}</p>
      <p>{{this.occupiedPercentage}}</p>
    </section>
    <section class="shelters">
      <div class="sector" v-for='(sector, i) in typesOfShelters' :key='i'>
        <SectorList :sector="sector" :i="i" :sheltersOrganized="sheltersOrganized[i]" />
      </div>
    </section>
  </div>
</template>

<script>
import SectorList from '../components/SectorList.vue'
import axios from 'axios'
export default {
  name: 'home',
  components: {
    SectorList
  },

  data() {
    return {
      error: false,
      info: null,
      loading: true,
      months: [
        'Jan',
        'Feb',
        'March',
        'April',
        'May',
        'June',
        'July',
        'Aug',
        'Sept',
        'Oct',
        'Nov',
        'Dec'
      ],
      typesOfShelters: [],
    }
  },

  mounted () {
    axios.get('https://secure.toronto.ca/c3api_data/v2/DataAccess.svc/ssha/extractssha?$format=application/json;odata.metadata=none&$top=1000&$orderby=OCCUPANCY_DATE desc')
      .then(response => {
        this.info = response.data.value
        this.typesOfShelters = [...new Set(response.data.value.map(s => s.SECTOR))]
        this.show = this.typesOfShelters.map(() => false)
      })
      .catch(error => {
        console.log(error)
        this.error = true
      })
      .finally(() => this.loading = false)
  },
  
  computed: {
    mostRecentOccupancyDate(){
      return new Date(Math.max.apply(null, this.info.map(function(e) {
        return new Date(e.OCCUPANCY_DATE);
      })));
    },

    currentData(){
      const startDate = this.mostRecentOccupancyDate
      const endDate = new Date()
      const currentData = this.info.filter(function (shelter) {
        const date = new Date(shelter.OCCUPANCY_DATE);
          return (date >= startDate && date <= endDate);
        });
      return currentData
    },

    sheltersOrganized(){
      return this.typesOfShelters.map(i => {
        return this.currentData.filter(s => s.SECTOR == i)
      })
    },

    totalBeds() {
      return this.currentData.reduce((acc, currentValue) => {
        return acc + currentValue.CAPACITY
      }, 0);
    },
    occupiedBeds() {
      return this.currentData.reduce((acc, currentValue) => {
        return acc + currentValue.OCCUPANCY
      }, 0);
    },
    occupiedPercentage(){
      let result = ((this.occupiedBeds / this.totalBeds)*100).toFixed(1)
      return `${result}%`
    },
  },

  methods: {
  
  }
}
</script>

<style>

.card {
  /* height: 100px; */
}
.fade-enter-active, .fade-leave-active {
  height: 100px;
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  height: 0px;
}
</style>
