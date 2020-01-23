<template>
  <div id="app">
    <div id="nav" class="wrapper">
      <nav class="nav-buttons">
        <router-link exact to="/">Home</router-link> |
        <router-link exact to="/list">List</router-link> |
        <router-link exact to="/about">About</router-link> 
      </nav>
    </div>
    <transition
        name="fade"
        mode="out-in"
      >
      <div class="loader" v-if="loading"></div>
      <router-view v-else :info="info" :typesOfShelters="typesOfShelters"/>
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
      })
      .catch(error => {
        // console.log(error)
        this.error = error
      })
      .finally(() => this.loading = false)
  },
}
</script>

<style scoped>
/* Nav Styles */
#nav {
  display: flex;
  justify-content: flex-end;
  box-shadow: 0 2px 2px -6px rgba(0,0,0,0.16);
  font-size:1.8rem;
}

#nav a {
  font-weight: bold;
  color: #3E00ED;
  text-decoration: none;
  transition:  color 200ms cubic-bezier(.4,.4,.25,1), background 200ms cubic-bezier(.4,.4,.25,1);
  border-bottom: 3px solid transparent;
}

#nav a:hover{
  border-bottom: 3px solid rgba(63, 0, 237, 0.541);
  transition: border 200ms cubic-bezier(.4,.4,.25,1)
}


#nav a.router-link-exact-active {
  border-bottom: 3px solid #3E00ED;
  transition: border 200ms cubic-bezier(.4,.4,.25,1)
}

nav {
  position: absolute;
  top: 8rem;
  right: 0;
  z-index: 10;
}

@media (max-width: 680px) {
  #nav {
    font-size: 1.6rem;
  }
}
/* end of Nav Styles */

</style>

