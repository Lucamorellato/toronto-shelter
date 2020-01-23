<template>
  <div class="wrapper">
      <h1>List{{$route.path.query}}</h1> 
      {{id}}
  </div>
</template>

<script>
export default {
    name: 'list',
    mounted(){
        // console.log(this.$route.path.query)
    },
    beforeRouteEnter (to, from, next) {
        next(vm => {
            vm.id = to.query.i
            // access to component instance via `vm`
        })
    },
    data(){
        return {
            id: null
        }
    }

    
}
</script>

<style>

</style>