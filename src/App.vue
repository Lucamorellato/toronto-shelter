<template>
  <div id="app">
    <transition
        name="fade"
        mode="out-in"
      >
    <div v-if="!loading" id="nav" class="wrapper">
      <nav class="nav-buttons">
        <router-link exact to="/">Home</router-link> |
        <router-link exact to="/list" :class="currentPage.includes('list') ? 'active' : null">List</router-link> |
        <router-link exact to="/about">About</router-link> 
      </nav>
    </div>
    </transition>
    <div class="loader" v-if="loading"></div>

    {{this.error}}
    <transition
      name="fade"
      mode="out-in"
    >
      <router-view v-if="!loading" :info="info" :typesOfShelters="typesOfShelters" :mostRecentOccupancyDate="mostRecentOccupancyDate" :currentData="info" :sheltersOrganized="sheltersOrganized" />
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
      error: null,
      info: null,
      loading: true,
      typesOfShelters: [],
    }
  },
  mounted() {
    axios.get('https://secure.toronto.ca/c3api_data/v2/DataAccess.svc/ssha/extractssha?$format=application/json;odata.metadata=none&$top=300&$orderby=OCCUPANCY_DATE desc')
      .then(response => {
        console.log('what')
        this.info = response.data.value
        this.typesOfShelters = [...new Set(response.data.value.map(s => s.SECTOR))]
        this.loading = false
      })
       .catch(error => {
        this.error = error.message
        console.log(error)
        this.$router.push('/notfound')
      })
      .finally(() => {
        this.loading = false
        console.log('FINALLY')}
      )
  },
  computed: {
    mostRecentOccupancyDate(){
      let rawDate = this.info[0].OCCUPANCY_DATE
      let year = parseInt(rawDate.slice(0,4))
      // -1 to date because January is 0 not 1
      let month = parseInt(rawDate.slice(5, 7) - 1)
      let day = parseInt(rawDate.slice(8, 10))

      return new Date(year, month, day)
    },
    sheltersOrganized(){
      return this.typesOfShelters.map(i => {
        return this.info.filter(s => s.SECTOR == i)
      })
    },
    currentPage(){
        return this.$route.path;
    },
  },
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


#nav a.router-link-active, #nav a.active {
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

