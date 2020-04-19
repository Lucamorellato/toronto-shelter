<template>
    <div v-show="show == i">
        <transition name="fade">
            <p class="top" v-show="show == i"><span class="big">{{sheltersOrganized.length + 1}}</span> shelters in "{{sector.toUpperCase()}}"</p>
        </transition>
        <transition name="accordion" v-on:before-enter="beforeEnter" v-on:enter="enter"
        v-on:before-leave="beforeLeave" v-on:leave="leave">
            <ul  v-show="show == i" class="accordion-content">
                <div v-for='(shelter, y) in sheltersOrganized' :key='y' >
                    <Card :shelter="shelter" />
                </div>
            </ul>
        </transition>
    </div>
</template>

<script>
import Card from '../components/Card.vue'
export default {
    name: 'SectorList',
    components: {
        Card,
    },
    props: {
        i: Number,
        show: Number,
        sector: String,
        sheltersOrganized: Array,
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
    },
    methods: {
        increaseDocumentSize(){
            const app =  document.getElementById(`app`);
            app.style.minHeight = '100vh';
        },
        beforeEnter: function(el) {
            el.style.height = '0';
        },
        enter: function(el) {
            el.style.height = el.scrollHeight + 'px';
            el.style.opacity = '1';
        },
        beforeLeave: function(el) {
            el.style.height = el.scrollHeight + 'px';
        },
        leave: function(el) {
            el.style.height = '0';
            el.style.opacity = '0';
        }
    }
}
</script>

<style scoped>

.top {
    font-size: 2rem;
    margin: 3rem 0;
}

.big {
    font-size: 3rem;
    font-weight: 900;
}


.accordion-content{
    transition: 750ms cubic-bezier(.4,.4,.25,1);
    overflow: hidden;
    width: 100%;
    background: white;
}

.accordion-content>*{
    width: 100%;
}

@media (max-width: 728px) {
    .accordion-content {
        background: rgba(255, 255, 255, 0.719);
    }
}

</style>