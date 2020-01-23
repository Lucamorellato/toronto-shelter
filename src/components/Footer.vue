<template>
    <footer>
        <p>©{{new Date().getFullYear()}} <a href="https://www.lucamorellato.com">Luca Morellato ^</a></p>
    </footer>
</template>

<style>
    footer {
        position: absolute;
        bottom: 0;
        width: 100%;
        text-align: center;
        /* background:rgb(247, 247, 247); */
        margin-top: 5rem;
        color: black;
    }
    footer a {
        text-decoration: none;
        color: black;
        border-bottom: 3px solid transparent;
    }
    footer a:hover {
        border-bottom: 3px solid black;
        transition: border 200ms cubic-bezier(.4,.4,.25,1)
    }
</style>