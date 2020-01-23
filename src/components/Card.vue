<template>
    <li class="card" :style="`background: ${capacityColor}`">
        {{shelter.SHELTER_NAME}}
        {{shelter.PROGRAM_NAME}}
        {{shelter.OCCUPANCY}} of {{shelter.CAPACITY}} Beds
        <a :href="`https://maps.google.com/?q=${shelter.SHELTER_ADDRESS}, ${shelter.SHELTER_CITY}, ${shelter.SHELTER_POSTAL_CODE}`" target="_blank">Open in Maps <img class="map-marker" :src="require(`@/assets/map-marker.png`)" alt=""></a> 
    </li>
</template>

<script>
export default {
name: 'SectorList',
props: {
    shelter: Object,
},
computed: {
    occupiedPercentage(){
      let result = ((this.shelter.OCCUPANCY / this.shelter.CAPACITY)*100).toFixed(1)
      return result
    },
    capacityColor(){
        let capacity = parseInt(this.occupiedPercentage)
        if (capacity > 99) {
            return 'red'
        } else if (capacity >= 95) {
            return 'orange'
        } else if (capacity >= 80) {
            return 'yellow'
        } else {
            return '#613144'
        }
    }
},
methods: {
  
  }
}
</script>

<style scoped>

.card {
    margin-bottom: 40px;
    width: 70%;
    border-radius: 3px;
    box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
    padding: 15px;
    font-size: 18px;
}

.map-marker {
    width: 20px;
    height: 20px;
}

</style>