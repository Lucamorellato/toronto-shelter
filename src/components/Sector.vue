<template>
<div class="sector">
    <Graph :occupiedPercentage='occupiedPercentage' :sector='sector' :capacityColor='capacityColor' />
    <button @click='() => handleClick(i)' class="sector-button">{{sector.toUpperCase()}}</button>
</div>
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

.sector {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}


@media (max-width: 680px) {
    .sector {
      width: 70%;
      margin: 0 auto;
      border-bottom: 4px solid #e8e7e7;
    }
}
@media (max-width: 480px) {
    .sector {
      width: 90%;
    }
}

.sector-button {
    background: #e8e7e7;
    border-radius: 15px;
    padding: 10px 15px;
    font-size: 25px;
    border: none;
    transition: background 0.15s cubic-bezier(.4,.4,.25,1), color 0.15s cubic-bezier(.4,.4,.25,1);
    /* display: block; */
    margin: 2rem 0;
}

.sector-button:hover{
    background:black;
    color: #e8e7e7;
    cursor: pointer;
}

</style>