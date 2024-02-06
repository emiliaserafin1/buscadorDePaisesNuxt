<template>
    <div class="modal">
        <div class="btn-container">
            <button id="cerrar">x</button>
        </div>
        <div class="modal-content">
            <div class="dialog-cabecera">
                <h2>{{ pais.name.common }}</h2>
                <img :src="pais.flags.png" :alt="pais.flags.alt">
            </div>
            <div class="dialog-info">
                <p v-if="pais.capital">Capital: <span>{{ pais.capital.join(' ') }}</span></p>
                <p v-else>Capital: No tiene</p>
                <p>Población: <span>{{ pais.population }}</span></p>
                <p>Continente <span>{{ pais.continents.join(' ') }}</span></p>
                <p>Area: <span>{{ pais.area }}</span></p>
                <p>Densidad: <span>{{ pais.population / pais.area }}</span></p>
                <p v-if="pais.currencies">Moneda: <span>{{ Object.keys(pais.currencies).map(clave => pais.currencies[clave].name).join(", ") }}</span></p>
                <p v-else>Monedas: No tiene</p>
                <a :href="pais.maps.googleMaps"></a>
                <p>ONU: <span>{{ pais.unMember }}</span>`</p>
                <p v-if="pais.languages">Idiomas: <span>{{ Object.keys(pais.languages).map(clave => pais.languages[clave]).join(", ") }}</span></p>
                <p v-else>Idioma: <span>No tiene</span></p>
            </div>
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
  }
}
</script>

<style scoped>
dialog {
    overflow: hidden;
    margin: auto;
    border-radius: 10px;
    border: none;
    box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    -webkit-box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    -moz-box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
}

.btn-container {
    width: 100%;
    display: flex;
    justify-content: end;
}

.modal-content {
    margin: 0px 30px 30px 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    color: #464646;
    h2 {
        font-size: 30px;
        margin: 10px 0 0 10px;
    }
    img {
        margin: 10px 20px 0 0;
        width: 350px;
        object-fit: cover;
    }
}

#cerrar {
    font-size: 20px;
    margin: 5px 15px;
    background: none;
    border: none;
    cursor: pointer;
}
    
</style>
