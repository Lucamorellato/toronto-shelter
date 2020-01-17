<template>
  <div class="home">
    <h1>Toronto Shelter Occupancy</h1>
    <div class="loader" v-if="loading">loading</div>
    <div class="content" v-if="!loading">
      <h2>{{this.months[`${new Date().getMonth()}`]}} {{new Date().getDate()}} Statistics:</h2>
      <p>{{this.occupiedBeds}} / {{this.totalBeds}}</p>
      <p>{{this.capacityPercentage}}</p>

    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
export default {
  name: 'home',

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
      ]
    }
  },

  mounted () {
    axios.get('https://secure.toronto.ca/c3api_data/v2/DataAccess.svc/ssha/extractssha?$format=application/json;odata.metadata=none&$top=1000&$orderby=OCCUPANCY_DATE desc')
      .then(response => {
        this.info = response.data.value
      })
      .catch(error => {
        console.log(error)
        this.errored = true
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

    typesOfShelters(){
      return [...new Set(this.currentData.map(s => s.SECTOR))];
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
    capacityPercentage(){
      let result = ((this.occupiedBeds / this.totalBeds)*100).toFixed(1)
      return `${result}%`
    },
  },

  methods: {
    
  }
}
</script>
