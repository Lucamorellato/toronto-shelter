<template>
    <footer>
        <p>©{{new Date().getFullYear()}} <a href="https://www.lucamorellato.com">Luca Morellato</a> -- Design: <a title="-♡-" href="https://www.linkedin.com/in/candicecloutier/">Candice Cloutier</a></p>
    </footer>
</template>

<style>

footer {       
    height: 50px;
    width: 100%;
    text-align: center;
    color: #274490;
    font-size: 1.6rem;
}

footer a {
    text-decoration: none;
    color: #274490;
    border-bottom: 3px solid transparent;
}

footer a:hover {
    border-bottom: 3px solid rgba(39, 68, 144,0.8);
    transition: border 200ms cubic-bezier(.4,.4,.25,1)
}

@media (max-width: 615px) {
    footer {
        font-size: 1.2rem
    }
}

</style>