<template>
   <div class="wrapper">
      <h1>List</h1> 
      <button v-for="(type, i) in typesOfShelters2" :key="i" :class="show == i ? 'active' : null" @click="handleChange(i)">{{type}}</button>
      <div v-for='(sector, index, name) in typesOfShelters2' :key='name'>
         <SectorList key="index" :i='index' :show='parseInt(show)' :sheltersOrganized='index === typesOfShelters2.length - 1 ? currentData : sheltersOrganized[index]' :sector='sector' />
      </div>
  </div>
</template>

<script>
import SectorList from '../components/SectorList.vue';
export default {
    name: 'list',
    props: {
        typesOfShelters: Array,
        currentData: Array,
        mostRecentOccupancyDate: Date,
        sheltersOrganized: Array,
    },
    components: {
        SectorList,
    },
    data(){
        return {
            show: null,
            typesOfShelters2: [],
        }
    },
    beforeRouteEnter (to, from, next) {
        next(vm => {
            if (!to.query.i) {
                // this sets id to the length of the Array, which is equal to the last index or 'ALL'
                vm.show = vm.typesOfShelters.length
           } else { 
               let id = parseInt(to.query.i)
               vm.show = id
           }
        })
    },
    mounted(){
        this.typesOfShelters2 = [...this.typesOfShelters, 'All']
    },
    methods: {
        handleChange(e){
            this.show = e
            this.$router.push(`?i=${e}`)
        }
    },
}
</script>

<style scoped>

button.active {
   background: red;
}

</style>