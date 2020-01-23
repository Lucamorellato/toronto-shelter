<template>
    <router-link exact :to="`/list?i=${i}`" class="sector-card">
        <h4>{{sector.toUpperCase()}}</h4>
        <p>{{occupiedBeds}} of {{totalBeds}} beds</p>
        <Graph :occupiedPercentage='occupiedPercentage' :sector='sector' :capacityColor='capacityColor' />
        <p class="link" @click='() => handleClick(i)'>View availability ></p>
    </router-link>
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

.sector-card {
    display: flex;
    flex-direction: column;
    color: #3e00ed;
    background: white;
    text-decoration: none;
    box-shadow: 0 2px 6px rgba(62, 0, 237,0.25), 0 2px 6px rgba(62, 0, 237,0.3);
    transition: box-shadow 0.15s cubic-bezier(.4,.4,.25,1);
    padding: 10px 20px;
    min-height: 400px;
    width:300px;
    border-radius: 2px;
    margin: 2rem 0;
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
    font-size: 1.8rem;
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
    border-bottom: 3px solid #3e00ed;
}


@media (max-width: 680px) {
    .sector-card {
        margin: 0;
    }
    
}
@media (max-width: 400px) {
   .sector-card {
       width: 100%;
       min-height: 375px;
   }
   .link {
       width: 70%;
   }
}


</style>