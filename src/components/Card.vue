<template>
    <li class="card">
        <div class="card-content">
            <p class="name">{{shelter.SHELTER_NAME}}</p>
            <p>{{shelter.PROGRAM_NAME}}</p>
            <p class="address">{{shelter.SECTOR}} - {{shelter.SHELTER_ADDRESS}}</p>
            <p class="beds" :class="capacityColor">{{shelter.OCCUPANCY}} of {{shelter.CAPACITY}} Beds <br/><span v-if="capacityColor === 'red'">At Capacity</span></p>
            <a :href="`https://maps.google.com/?q=${shelter.SHELTER_ADDRESS}, ${shelter.SHELTER_CITY}, ${shelter.SHELTER_POSTAL_CODE}`" target="_blank"><img class="map-marker" :src="require(`@/assets/map-marker.png`)" alt="">Open in Maps </a> 
        </div>
        <span class="divider"></span>
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
        if (capacity >= 98) {
            return 'red'
        } else if (capacity >= 97) {
            return 'orange'
        } else if (capacity >= 85) {
            return 'yellow'
        } else {
            return 'normal'
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
    border-radius: 3px;
    font-size: 18px;
    margin: 0 auto;
    position: relative;
}

.card-content {
    padding:4rem;
}

.card-content p.name {
    font-weight: 900;
    font-size: 1.8rem;
}

.card-content p {
    margin: 0;
}

.card a {
    color: #274490;
}

.beds {
    position: absolute;
    top: 5.6rem;
    right: 4rem;
}
.beds.red {
    color:  rgb(245, 70, 20);
}
.beds.orange {
    color: rgb(245, 146, 25);
}
.beds.yellow {
    color: rgb(250, 230, 0);
}

.divider {
    display: block;
    height: 5px;
    width: 75%;
    background:#fff0df;
    margin: 0 auto;
}

.map-marker {
    width: 20px;
    height: 20px;
    margin-top:5px;
}

@media (max-width: 615px) {

    .card p {
        font-size: 1.6rem;
    }
    .address, .beds span {
        display: none;
    }
    .beds {
        position: static;
    }
  
}


</style>