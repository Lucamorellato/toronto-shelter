<template>
  <div class="home wrapper">
    <section class="hero-content">
      <h1>Shelter <br/>Occupancy TO</h1>
      <h2>{{this.months[`${new Date().getMonth()}`]}} {{new Date().getDate() - 1}}</h2>
    </section>
    <section class="sectors">
        <Sector sector='All' :i='typesOfShelters.length' :sheltersOrganized='this.currentData' @click="handleClick" />
        <Sector v-for='(sector, i) in typesOfShelters' :key='i' :sector='sector' :i='i' :sheltersOrganized='sheltersOrganized[i]' @click="handleClick" />
    </section>
    <!-- <section class="shelters">
      <div v-for='(sector, i) in typesOfShelters' :key='i'>
         <SectorList :sector='sector' :i='i' :show='show' :sheltersOrganized='sheltersOrganized[i]' />
      </div>
    </section> -->
  </div>
</template>

<script>
import Sector from '../components/Sector.vue'

export default {
  name: 'home',
  components: {
    Sector,
  },
  props: {
    info: Array,
    typesOfShelters: Array,
    currentData: Array,
    mostRecentOccupancyDate: Date,
    sheltersOrganized: Array,
    handleClick: Function,
  },

  data() {
    return {
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
      show: [null],
    }
  },

  computed: {

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
    capacityColor(){
      let capacity = parseInt(this.occupiedPercentage)
      if (capacity >= 98) {
        return 'red'
      } else if (capacity >= 95) {
          return 'orange'
      } else if (capacity >= 85) {
          return 'yellow'
      } else {
          return 'normal'
      }
    }
  },
  methods: {
   
  },
}
</script>

<style scoped>

.sectors {
  width: 100%;
  padding: 8rem 0 12rem 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
}

@media (max-width: 1128px) {
  .sectors {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 3rem 2rem;
    justify-items: center;
  }
}
@media (max-width: 728px) {
  .sectors {
    /* grid-template-columns: 1fr; */
    grid-gap: 5rem 0;
  }
}

@media (max-width: 615px) {
  .sectors {
    grid-template-columns: 1fr;
    /* grid-gap: 5rem 0; */
  }
}


</style>
