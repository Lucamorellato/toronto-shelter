<template>
  <div class="wrapper">
      <h1>List</h1> 
      <button v-for="(type, i) in typesOfShelters2" :key="i">{{type}}</button>
      {{id}}
      <SectorList :i="parseInt(id)" :sheltersOrganized="sheltersOrganized" />
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
    
    beforeRouteEnter (to, from, next) {
        next(vm => {
            console.log(to.query, vm.id, from, 'from')
        //     if (!to.query.i) {
        //         let id = vm.typesOfShelters.length - 1
        //         vm.id = id
        //         // access to component instance via `vm`
        //    } else { 
        //        let id = to.query.i
        //        vm.id = id
        //    }
        })
    },
    mounted(){
        this.typesOfShelters2 = [...this.typesOfShelters, 'All'].reverse()
    },

    data(){
        return {
            id: null,
            typesOfShelters2: [],
        }
    }

    
}
</script>

<style>

</style>