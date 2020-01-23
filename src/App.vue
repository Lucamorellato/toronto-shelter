<template>
  <div id="app">
    <transition
        name="fade"
        mode="out-in"
      >
    <div v-if="!loading" id="nav" class="wrapper">
      <nav class="nav-buttons">
        <router-link exact to="/">Home</router-link> |
        <router-link exact to="/list">List</router-link> |
        <router-link exact to="/about">About</router-link> 
      </nav>
    </div>
    </transition>
    <div class="loader" v-if="loading"></div>
    <transition
        name="fade"
        mode="out-in"
      >
      <router-view v-if="!loading" :info="info" :typesOfShelters="typesOfShelters" :mostRecentOccupancyDate="mostRecentOccupancyDate" :currentData="currentData" :sheltersOrganized="sheltersOrganized" :handleClick="handleClick" />
    </transition>
    <transition
        name="fade"
        mode="out-in"
      >
    <Footer v-if='!loading'/>
    </transition>
  </div>
</template>

<script>
import Footer from './components/Footer.vue'
import axios from 'axios'
export default {
  components: {
    Footer
  },
  data() {
    return {
      error: false,
      info: null,
      loading: true,
      typesOfShelters: [],
    }
  },
  mounted () {
    axios.get('https://secure.toronto.ca/c3api_data/v2/DataAccess.svc/ssha/extractssha?$format=application/json;odata.metadata=none&$top=1000&$orderby=OCCUPANCY_DATE desc')
      .then(response => {
        this.info = response.data.value
        this.typesOfShelters = [...new Set(response.data.value.map(s => s.SECTOR))]
        this.loading = false
      })
      .catch(error => {
        // console.log(error)
        this.error = error
      })
      .finally(() => this.loading = false)
  },
  computed: {
    mostRecentOccupancyDate(){
      return new Date(Math.max.apply(null, this.info.map(function(e) {
        return new Date(e.OCCUPANCY_DATE);
      })));
    },

    // THIS IS THE CORRECT WAY TO FILTER BY OCCUPANCY_DATE which is not currently being updated by the API
    currentData(){
      const startDate = this.mostRecentOccupancyDate
      const endDate = new Date()
      const currentData = this.info.filter(function (shelter) {
        const date = new Date(shelter.OCCUPANCY_DATE);
          return (date >= startDate && date <= endDate);
        });
      return currentData
    },

    sheltersOrganized(){
      return this.typesOfShelters.map(i => {
        return this.currentData.filter(s => s.SECTOR == i)
      })
    },
  },
  methods: {
    handleClick(){
      console.log('fuc')
    }
  }
}
</script>

<style scoped>
/* Nav Styles */
#nav {
  display: flex;
  justify-content: flex-end;
  box-shadow: 0 2px 2px -6px rgba(0,0,0,0.16);
  font-size:2.2rem;
  color: rgb(23, 23, 23);
}

#nav a {
  color: rgb(23, 23, 23);
  font-weight: bold;
  text-decoration: none;
  transition:  color 200ms cubic-bezier(.4,.4,.25,1), background 200ms cubic-bezier(.4,.4,.25,1);
  border-bottom: 3px solid transparent;
  font-weight: 900;
  text-transform: uppercase;
}

#nav a:hover{
  border-bottom: 3px solid rgba(23, 23, 23, 0.541);
  transition: border 200ms cubic-bezier(.4,.4,.25,1)
}


#nav a.router-link-exact-active {
  border-bottom: 3px solid rgb(23, 23, 23);
  transition: border 200ms cubic-bezier(.4,.4,.25,1)
}

nav {
  position: absolute;
  top: 8rem;
  z-index: 10;
}

@media (max-width: 680px) {
  #nav {
    font-size: 1.6rem;
  }
  nav {
    top: 2rem;
  }
}
/* end of Nav Styles */

</style>

