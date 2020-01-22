<template>
  <div id="app">
    <div id="nav">
      <router-link exact to="/">Home</router-link> |
      <router-link exact to="/about">About</router-link>
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
      <Footer v-if="this.$route.name == 'about'" />
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
  padding: 2rem;
  font-size: 1.2rem;
  background: darkgrey;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: rgb(41, 204, 84);
}
/* end of Nav Styles */

</style>

