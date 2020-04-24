<template>
    <section class="error">
        <h1>Uh Oh.</h1>
        <h2>Somehow I messed up and caused an error. </h2>
        <p>Please try again later. Or try navigating <router-link exact to="/">Home</router-link></p>
    </section>
</template>

<script>
export default {
    name: 'error',
}
</script>

<style scoped>  

h1 {
    padding-bottom: 10rem;
}

h2 {
    font-size: 2.2rem;
    font-weight: 500;
}

a {
    color: #274490;
    font-weight: bold;
    text-decoration: none;
    transition:  color 200ms cubic-bezier(.4,.4,.25,1), background 200ms cubic-bezier(.4,.4,.25,1);
    border-bottom: 3px solid transparent;
    font-weight: 900;
    text-transform: uppercase;
}

a:hover{
    border-bottom: 3px solid rgba(39, 68, 144, 0.54);
    transition: border 200ms cubic-bezier(.4,.4,.25,1)
}

</style>