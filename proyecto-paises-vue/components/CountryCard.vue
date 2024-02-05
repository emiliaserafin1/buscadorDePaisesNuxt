<script>
    export default {
        props: {
            pais: {
                type: Object,
                required: true
            }
        }
    }
</script>

<template>
    <div class="card">
        <div class="card-img">
            <img src="" alt="">
        </div>
        <div class="card-body">
            <h2>{{ pais.name.common }}</h2>
            <p>Capital: <span>No tiene</span>` : capital.innerHTML = `Capital: <span>{{ pais.capital }}</span></p>
            <p>Población: <span>{{ pais.population }}</span></p>
            <p>Continente: {{ continentesHTML }}</p>
        </div>
    </div>
</template>

<style scoped>

</style>
