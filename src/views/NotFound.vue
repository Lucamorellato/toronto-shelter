<template>
    <section class="notFound">
        <h1>Page Not Found</h1>
        <h2>Please try a differnet page</h2>
    </section>
</template>

<script>
export default {
    name: 'notfound',
}
</script>

<style scoped>  
    .notFounds {
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }

    h1 {
        padding-bottom: 10rem;
    }
    h2 {
        font-size: 2.2rem;
        font-weight: 500;
    }

</style>