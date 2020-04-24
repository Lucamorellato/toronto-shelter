<template>
   <div>
      <h1>List</h1> 
      <div class="button-container">
        <button v-for="(type, i) in typesOfShelters2" :key="i" :class="show == i ? 'active' : null" @click="handleChange(i)">{{type}}</button>
      </div>
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

button {
    cursor:pointer;
    background: white;
    border: 1px solid #274490;
    color: #274490;
    text-transform: uppercase;
    border-radius: 20px;
    text-align: center;
    display:inline-block;
    height: 30px;
    padding: 0 10px;
    max-width:150px;
    font-size: 1.6rem;
    margin-right: 1rem;
    transition: color 0.15s cubic-bezier(.4,.4,.25,1), background 0.15s cubic-bezier(.4,.4,.25,1);
}

button.active, button:hover {
   background: #274490;
   color: white;
}

.button-container {
    margin-top: 3rem;
}

@media (max-width: 550px) {

    .button-container {
        margin-top: 2rem;
    }
    button {
        margin-top: 1rem;
        margin-right: 1.5rem;
    }
}

</style>