<template>
  <div id="app" class="wrapper">
    <div id="nav">
      <span class="nav-buttons">
        <router-link exact to="/">Home</router-link> |
        <router-link exact to="/about">About</router-link>
      </span>
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
  font-size: 16px;
  display: flex;
  justify-content: flex-end;
  position: sticky;
  top: 1.2rem;
  box-shadow: 0 2px 2px -6px rgba(0,0,0,0.16);


}
#nav a {
  font-weight: bold;
  color: #165788;
  text-decoration: none;
  transition:  color 200ms cubic-bezier(.4,.4,.25,1), background 200ms cubic-bezier(.4,.4,.25,1);
}

#nav .nav-buttons {
  padding-right: 2rem;
}

#nav a.router-link-exact-active {
  border-bottom: 3px solid #165788;
}
#nav a.name {
  font-size: 22px;
  text-decoration: none;
  color: #165788;
  background: #fcfcfc;
  position: static;
}
/* end of Nav Styles */

</style>

