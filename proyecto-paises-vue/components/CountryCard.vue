<template>
    <div class="card">
        <div class="card-img">
            <img :src="pais.flags.png" :alt="pais.flags.alt">
        </div>
        <div class="card-body">
            <h2>{{ pais.name.common }}</h2>
            <p>Capital: <span>{{ pais.capital.join(' ') }}</span></p>
            <p>Población: <span>{{ pais.population }}</span></p>
            <p>Continente: {{ pais.continents.join(' ') }}</p>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            pais: {
                type: Object,
                required: true
            }
        },
    }
</script>

<style scoped>
    .card {
    margin: 30px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    width: 250px;
    height: 350px;
    border-radius: 5px;
    box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    -webkit-box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    -moz-box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    transition: transform 0.5s ease-out;
    img {
        width: 100%;
        height: 170px;
        object-fit: cover;
        border-bottom: 1px solid #464646;
    }   
    }

    .card:hover{
        transform: translate(0px, -8px); 
    }

    .card-body {
        margin: 30px 15px;
    }

    .card-body h2 {
        font-size: 18px;
        margin: auto;    
        font-weight: 600;
        margin-bottom: 5px;
    }
</style>
