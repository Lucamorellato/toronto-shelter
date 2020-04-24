<template>
  <div id="app">
    <div class="content">
      <transition
          name="fade"
          mode="out-in"
        >
      <div v-if="!loading" id="nav-container" class="wrapper">
        <nav class="nav-buttons" v-if="!currentPage.includes('error')">
          <router-link exact to="/">Home</router-link> |
          <router-link exact to="/list" :class="currentPage.includes('list') ? 'active' : null">List</router-link> |
          <router-link exact to="/about">About</router-link> 
        </nav>
      </div>
      </transition>
      <div class="loader" v-if="loading"></div>
      <transition
        name="fade"
        mode="out-in"
      >
        <router-view class="wrapper" v-if="!loading"   :typesOfShelters="typesOfShelters" :mostRecentDate="mostRecentDate" :currentData="currentData" :sheltersOrganized="sheltersOrganized" />
      </transition>
    </div>
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
      mostRecentDate: '',
    }
  },
  mounted() {
    axios.get('https://secure.toronto.ca/c3api_data/v2/DataAccess.svc/ssha/extractssha?$format=application/json;odata.metadata=none&$top=160&$orderby=OCCUPANCY_DATE desc')
      .then(response => {
        this.info = response.data.value
        this.typesOfShelters = [...new Set(response.data.value.map(s => s.SECTOR))]
        this.mostRecentDate = this.getMostRecentDate(this.info[0].OCCUPANCY_DATE)
      })
       .catch(error => {
        this.error = error.message
        this.info = []
        this.$router.push('/error')
      })
      .finally(() => this.loading = false)
  },
  computed: {
    currentData(){
      let startDate = this.info[0].OCCUPANCY_DATE
      
      return this.info.filter(s => s.OCCUPANCY_DATE === startDate )
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
  methods: {
    getMostRecentDate(date){
      let year = parseInt(date.slice(0,4))
      // -1 to date because January is 0 not 1
      let month = parseInt(date.slice(5, 7) - 1)
      let day = parseInt(date.slice(8, 10))

      return new Date(year, month, day)
    }
  }
}
</script>

<style scoped>

/* Nav Styles */
#nav-container {
  display: flex;
  justify-content: flex-end;
  box-shadow: 0 2px 2px -6px rgba(0,0,0,0.16);
  font-size:2.2rem;
  color: #274490;
}

#nav-container a {
  color: #274490;
  font-weight: bold;
  text-decoration: none;
  transition:  color 200ms cubic-bezier(.4,.4,.25,1), background 200ms cubic-bezier(.4,.4,.25,1);
  border-bottom: 3px solid transparent;
  font-weight: 900;
  text-transform: uppercase;
}

#nav-container a:hover{
  border-bottom: 3px solid rgba(39, 68, 144, 0.54);
  transition: border 200ms cubic-bezier(.4,.4,.25,1)
}


#nav-container a.router-link-active, #nav-container a.active {
  border-bottom: 3px solid #274490;
  transition: border 200ms cubic-bezier(.4,.4,.25,1)
}

nav {
  position: absolute;
  top: 8.6rem;
  z-index: 10;
}

@media (max-width: 680px) {
  #nav-container {
    font-size: 1.6rem;
  }
  nav {
    top: 2rem;
  }
}
/* end of Nav Styles */

</style>

