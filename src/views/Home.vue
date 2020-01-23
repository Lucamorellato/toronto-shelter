<template>
  <div class="home">
    <section class="hero-content">
      <h1>Toronto Shelter Occupancy</h1>
      <h2>{{this.months[`${new Date().getMonth()}`]}} {{new Date().getDate() - 1}} Shelter Capacity:</h2>
      <Graph :occupiedPercentage='occupiedPercentage' :sector='sector' :capacityColor='capacityColor' />
      <p class="numbers">{{this.occupiedBeds}} / {{this.totalBeds}} Beds</p>
    </section>
    <section class="sect">
      <h3>Sectors</h3>
      <div class="sectors">
        <Sector v-for='(sector, i) in typesOfShelters' :key='i' :sector='sector' :i='i' :sheltersOrganized='sheltersOrganized[i]' :handleClick='handleClick' />
      </div>
    </section>
    <section class="shelters">
      <div v-for='(sector, i) in typesOfShelters' :key='i'>
         <SectorList :sector='sector' :i='i' :show='show' :sheltersOrganized='sheltersOrganized[i]' />
      </div>
    </section>
  </div>
</template>

<script>
import Sector from '../components/Sector.vue'
import SectorList from '../components/SectorList.vue'
import Graph from '../components/Graph.vue'

export default {
  name: 'home',
  components: {
    Sector,
    SectorList,
    Graph
  },
  props: {
    info: Array,
    typesOfShelters: Array,
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
    mostRecentOccupancyDate(){
      return new Date(Math.max.apply(null, this.info.map(function(e) {
        return new Date(e.OCCUPANCY_DATE);
      })));
    },

    // THIS IS THE CORRECT WAY TO FILTER BY OCCUPANCY_DATE which is not currently being updated by the API
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
    handleClick(e){
      e === this.show[0] ? (this.show.pop(), this.show.push(null)) : (this.show.pop(), this.show.push(e));
    }
  },
}
</script>

<style scoped>
.home {
  max-width: 1280px;
  margin: 0 auto;
}

.hero-content {
  text-align: center;
  color: #165788;
  displaY: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.hero-content .numbers {
  font-size: 20px;
  font-weight: bold;
}

.sectors {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
}
@media (max-width: 1128px) {
  .sectors {
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 3rem 1rem;
  }
}
@media (max-width: 680px) {
  .sectors {
    grid-template-columns: 1fr;
    grid-gap: 3rem;
  }
}

</style>
