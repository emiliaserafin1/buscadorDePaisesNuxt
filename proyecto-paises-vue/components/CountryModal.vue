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
                <p v-if="pais.capital">Capital: <span>{{ capitalText }}</span></p>
                <p v-else>Capital: No tiene</p>
                <p>Población: <span>{{ pais.population }}</span></p>
                <p>Continente: <span>{{ continentsText }}</span></p>
                <p>Area: <span>{{ pais.area }}</span></p>
                <p>Densidad: <span>{{ density }}</span></p>
                <p v-if="pais.currencies">Moneda: <span>{{ currencyText }}</span></p>
                <p v-else>Monedas: No tiene</p>
                <a :href="pais.maps.googleMaps"></a>
                <p>ONU: <span>{{ pais.unMember }}</span>`</p>
                <p v-if="pais.languages">Idiomas: <span>{{ languageText }}</span></p>
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
    },
    computed: {
        capitalText() {
            return this.pais.capital ? this.pais.capital.join(' ') : '';
        },
        continentsText() {
            return this.pais.continents ? this.pais.continents.join(' ') : '';
        },
        density() {
            return this.pais.population / this.pais.area;
        },
        currencyText() {
            if (this.pais.currencies) {
                return Object.keys(this.pais.currencies).map(clave => this.pais.currencies[clave].name).join(", ");
            } else {
                return '';
            }
        },
        languageText() {
            if (this.pais.languages) {
                return Object.keys(this.pais.languages).map(clave => this.pais.languages[clave]).join(", ");
            } else {
                return '';
            }
        }
    }
}
</script>


<style scoped>
.modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 9999;
    margin: auto;
    border-radius: 10px;
    border: none;
    box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    -webkit-box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    -moz-box-shadow: -2px 4px 14px -1px rgba(0,0,0,0.57);
    background-color: white;
}

.btn-container {
    width: 100%;
    display: flex;
    justify-content: flex-end;
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
