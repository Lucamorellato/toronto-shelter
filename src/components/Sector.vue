<template>
<!-- <div class="card-container"> -->
    <router-link exact :to="`/list?i=${i}`" class="sector-card">
        <h4>{{sector.toUpperCase()}}</h4>
        <p>{{occupiedBeds}} of {{totalBeds}} beds</p>
        <Graph :occupiedPercentage='occupiedPercentage' :sector='sector' :capacityColor='capacityColor' />
        <p class="link" @click='() => handleClick(i)'>View availability ></p>
    </router-link>
<!-- </div> -->
</template>

<script>

import Graph from '../components/Graph.vue'
export default {
    props: {
        sector: String,
        i: Number,
        sheltersOrganized: Array,
        handleClick: Function,
    },
    components: {
        Graph,
    },
    computed: {
        totalBeds() {
            return this.sheltersOrganized.reduce((acc, currentValue) => {
            return acc + currentValue.CAPACITY
            }, 0);
        },
        occupiedBeds() {
            return this.sheltersOrganized.reduce((acc, currentValue) => {
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
            } else if (capacity >= 97) {
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

.card-container {
    width: calc(33% - 2rem); 
}

.sector-card {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    color: #3e00ed;
    background: white;
    text-decoration: none;
    box-shadow: 0 3px 7px rgba(62, 0, 237,0.15), 0 3px 8px rgba(62, 0, 237,0.18);
    transition: box-shadow 0.15s cubic-bezier(.4,.4,.25,1);
    padding: 10px 20px;
    min-height: 475px;
    width: calc(33% - 2rem); 
    /* max-width: 350px; */
    border-radius: 2px;
    margin: 2rem 1rem;
}

h4, p {
    margin: 0;
}

h4 {
    font-size: 2.6rem;
    font-weight: bold;
}

p {
    font-size: 1.6rem;
}

.link {
    width: 60%;
    font-size: 1.6rem;
    padding-top: 1.5rem;
    text-align: center;
    margin: 0 auto;
    border-bottom: 3px solid transparent;
    transition: border 0.25s cubic-bezier(.4,.4,.25,1);
}
.sector-card:hover {
    box-shadow: 0 5px 8px rgba(62, 0, 237,0.25), 1px 2px 8px rgba(62, 0, 237,0.3);
}
.sector-card:hover .link {
    border-bottom: 3px solid rgba(62, 0, 237,0.8);
}

@media (max-width: 1128px) {
    .sector-card {
        width: 100%;
        min-height: 450px;
    }
}

@media (max-width: 728px) {
    .sector-card {
        margin: 0;
        min-width: 280px;
        width: 85%;
        min-height: 425px;
    }
}
@media (max-width: 625px) {


}
@media (max-width: 450px) {
   .sector-card {
       width: 100%;
   }
   .link {
       width: 70%;
   }
}


</style>