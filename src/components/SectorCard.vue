<template>
    <router-link exact :to="`/list?i=${i}`" class="sector-card">
        <div class="text">
            <h3>{{sector}}</h3>
            <p>{{occupiedBeds}} of {{totalBeds}} beds</p>
        </div>
        <Graph :occupiedPercentage='occupiedPercentage' :sector='sector' :capacityColor='capacityColor' />
        <p class="link">View availability ></p>
    </router-link>
</template>

<script>

import Graph from '../components/Graph.vue'
export default {
    props: {
        sector: String,
        i: Number,
        sheltersOrganized: Array,
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
    color: #274490;
    background: white;
    text-decoration: none;
    box-shadow: 0 3px 7px rgba(39, 68, 144,0.15), 0 3px 8px rgba(39, 68, 144,0.18);
    transition: box-shadow 0.15s cubic-bezier(.4,.4,.25,1);
    padding: 1rem 2rem;
    margin-bottom: 4rem;
    min-height: 475px;

    width: calc(33% - 2rem); 
    border-radius: 2px;
}

h3, p {
    margin: 0;
}

h3 {
    font-size: 2.6rem;
    font-weight: bold;
    text-transform: uppercase;
}

p {
    font-size: 1.6rem;
}

.link {
    width: 55%;
    font-size: 1.6rem;
    padding-top: 1.5rem;
    text-align: center;
    margin: 0 auto;
    border-bottom: 3px solid transparent;
    transition: border 0.25s cubic-bezier(.4,.4,.25,1);
}
.sector-card:hover {
    box-shadow: 0 5px 8px rgba(39, 68, 144,0.25), 1px 2px 8px rgba(39, 68, 144,0.3);
}
.sector-card:hover .link {
    border-bottom: 3px solid rgba(39, 68, 144, 0.8);
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
        width: 75%;
        min-height: 425px;
    }
}
/* @media (max-width: 625px) {
    

} */
@media (max-width: 615px) {
   .sector-card {
       width: 100%;
   }
   .link {
       width: 70%;
   }
}


</style>