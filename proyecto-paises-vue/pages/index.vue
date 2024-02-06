<template>
  <div class="ligth-mode">
    <div class="card-container">
      <div id="loader" class="loader"></div>
      <CountryCard v-for="pais in listaDePaisesAux" :pais="pais" :key="pais.cca3"></CountryCard>
    </div>
  </div>
</template>

<script>
import CountryCard from '~/components/CountryCard.vue';

export default {
  components: {
    CountryCard
  },
  
  data() {
    return {
      paisesApiUrl: 'https://restcountries.com/v3.1/all',
      filtrarPorContinenteApiUrl: 'https://restcountries.com/v3.1/region/',
      listaDePaisesAux: [],
    };
  },
  
  methods: {
    async obtenerPaises() {
      this.mostrarLoader();
      try {
        const response = await fetch(this.paisesApiUrl);
        const paises = await response.json();
        console.log('Paises:', paises);
        this.listaDePaisesAux = paises;
        this.ocultarLoader();
        return paises;
      } catch (error) {
        this.ocultarLoader();
      }
    },

    async filtrarPorContinente(continente) {
      this.mostrarLoader();
      try {
        const response = await fetch(this.filtrarPorContinenteApiUrl + continente);
        const paises = await response.json();
        this.listaDePaisesAux = paises;
        this.ocultarLoader();
        return paises;
      } catch (error) {
        this.ocultarLoader();
      }
    },

    mostrarLoader() {
      const loader = document.getElementById('loader');
      if (loader) {
        loader.style.display = 'block';
      }
    },

    ocultarLoader() {
      const loader = document.getElementById('loader');
      if (loader) {
        loader.style.display = 'none';
      }
    },
  },

  mounted() {
    this.obtenerPaises();
  },
};
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Roboto, sans-serif;
  }

  .card-container {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
  }

  .loader {
    border: 8px solid #f3f3f3;
    border-top: 8px solid #3498db;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    animation: spin 1s linear infinite;
    position: fixed;
    top: 50%;
    left: 50%;
    margin-top: -25px;
    margin-left: -25px;
    display: none;
  }

  @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
  }

</style>


