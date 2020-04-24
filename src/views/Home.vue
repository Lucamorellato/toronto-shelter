<template>
  <div class="home">
    <section class="hero-content">
      <h1>Shelter <br/>Occupancy TO</h1>
      <h2 class="date">Reported: {{this.months[`${this.mostRecentDate.getMonth()}`]}} {{this.mostRecentDate.getDate()}}</h2>
    </section>
    <section class="sectors">
        <SectorCard sector='All' :i='typesOfShelters.length' :sheltersOrganized='this.currentData'  />
        <SectorCard v-for='(sector, i) in typesOfShelters' :key='i' :sector='sector' :i='i' :sheltersOrganized='sheltersOrganized[i]'  />
    </section>
  </div>
</template>

<script>
import SectorCard from '../components/SectorCard.vue'

export default {
  name: 'home',
  components: {
    SectorCard,
  },
  props: {
    typesOfShelters: Array,
    currentData: Array,
    mostRecentDate: Date,
    sheltersOrganized: Array,
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
}
</script>

<style scoped>

.date {
  padding: 6rem 0 1rem 0;
}

.sectors {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 4rem;
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
