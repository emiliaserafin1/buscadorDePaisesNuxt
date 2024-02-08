<template>
  <div>
    <div class="filtros">
      <InputSearch @filtrarPorNombre="filter" ref="inputSearchRef"></InputSearch>
      <div  class="checkbox-container">
        <label for="filtro-paises"></label>
        <input id="filtro-paises" type="checkbox" v-model="checkBoxInd"/> Independiente
      </div>
      <div>
        <select name="orden" id="orden" v-model="ordenamiento">
          <option value="default" disabled selected>Ordenar por</option>
          <option value="nombre-asc">Nombre ascendente</option>
          <option value="nombre-desc">Nombre descendente</option>
          <option value="poblacion-asc">Población ascendente</option>
          <option value="poblacion-desc">Población descendente</option>
        </select>
        <select name="filtro" id="filtro" v-model="continente">
          <option value="default" selected disabled>Selecciona un continente</option>
          <option value="Europe">Europa</option>
          <option value="Americas">América</option>
          <option value="Oceania">Oceanía</option>
          <option value="Africa">África</option>
          <option value="Antarctic">Antártida</option>
          <option value="Asia">Asia</option>
        </select>
        <button @click="limpiarFiltros">Limpiar filtros</button>
      </div>
    </div>
    <div class="card-container">
      <div id="loader" class="loader"></div>
      <CountryCard v-for="pais in paisesFiltradosArr" :pais="pais" @click="mostrarModal(pais)"></CountryCard>
    </div>
  </div>
  <CountryModal v-if="showModal" :pais="selectedCountry" @cerrarModal="cerrarModal"></CountryModal>
</template>

<script>
import CountryCard from '~/components/CountryCard.vue';
import CountryModal from '~/components/CountryModal.vue';
import InputSearch from '~/components/InputSearch.vue';

export default {
  components: {
    CountryCard,
    CountryModal,
    InputSearch
  },
  
  data() {
    return {
      paisesApiUrl: 'https://restcountries.com/v3.1/all',
      buscarPorNombreUrl: 'https://restcountries.com/v3.1/name/',
      paisesIndUrl: 'https://restcountries.com/v3.1/independent?status=true',
      paisesFiltradosArr: [],
      inputValue: '',
      continente: 'default',
      ordenamiento: 'default',
      showModal: false,
      listaDePaisesAux: [],
      selectedCountry: {},
      paisesFiltradosArr: [],
      checkBoxInd: false 
    }
  },

  methods: {
    async obtenerPaises() {
      this.mostrarLoader();
      try {
        const response = await fetch(this.paisesApiUrl);
        const paises = await response.json();
        this.listaDePaisesAux = paises;
        this.paisesFiltradosArr = paises;
        this.ocultarLoader();
        return paises;
      } catch (error) {
        this.ocultarLoader();
      }
    },

    filter(inputValue) {
      this.inputValue = inputValue;
      this.filtrar();
    },

    async filtrarPorNombre() {
      try {
        const response = await fetch(this.buscarPorNombreUrl + this.inputValue);
        const paises = await response.json();
        return paises;
      } catch (error) {
        console.error('Error al filtrar por nombre:', error);
        return [];
      }
    },

    async filtrarIndependientes() {
      this.mostrarLoader();
      try {
        const response = await fetch(this.paisesIndUrl);
        const paises = await response.json();
        this.ocultarLoader();
        return paises;
      } catch (error) {
        console.error('Error al filtrar por independientes:', error);
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

    mostrarModal(pais) {
      this.selectedCountry = pais;
      this.showModal = true;
    },

    cerrarModal() {
      this.showModal = false;
      this.selectedCountry = null;
    },

    ordenNombreAsc(listaDePaises){
      listaDePaises.sort((a,b) => {
          if(a.name.common > b.name.common){
              return 1;
          }
          if(a.name.common < b.name.common){
              return -1;
          }
          return 0;
      });
      return listaDePaises;
    },

    ordenPoblacionAsc(listaDePaises){
      listaDePaises.sort((a,b) =>  a.population - b.population);
      return listaDePaises;
    },

    limpiarFiltros() {
      this.obtenerPaises();
      this.inputValue = '';
      this.$refs.inputSearchRef.inputSearch = '';
      this.continente = 'default';
      this.ordenamiento = 'default';
    },

    async filtrar() {
      let paisesFiltrados = [...this.listaDePaisesAux];

      if (this.inputValue.trim() !== '') {
        paisesFiltrados = await this.filtrarPorNombre();
      }

      if (this.checkBoxInd) {
        paisesFiltrados = await this.filtrarIndependientes();
      }

      if (this.continente !== 'default') {
        paisesFiltrados = paisesFiltrados.filter(pais => pais.region === this.continente);
      }
      
      if (this.ordenamiento === 'nombre-asc') {
        paisesFiltrados = this.ordenNombreAsc(paisesFiltrados);
      } else if (this.ordenamiento === 'nombre-desc') {
        paisesFiltrados = this.ordenNombreAsc(paisesFiltrados).reverse();
      } else if (this.ordenamiento === 'poblacion-asc') {
        paisesFiltrados = this.ordenPoblacionAsc(paisesFiltrados);
      } else if (this.ordenamiento === 'poblacion-desc') {
        paisesFiltrados = this.ordenPoblacionAsc(paisesFiltrados).reverse();
      }

      this.paisesFiltradosArr = paisesFiltrados;
    }
  },
  watch: {
    continente() {
        this.filtrar();
    },
    ordenamiento() {
        this.filtrar();
    },
    checkBoxInd() {
        this.filtrar();
    }
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

  .filtros {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 20px 40px;
    input, select, button {
      font-family: Roboto, sans-serif;
      height: 32px;
      border-radius: 5px;
      padding: 0 5px;
      font-size: 14px;
      cursor: pointer;
      margin: 0 3px;
    }
  }
  
  .checkbox-container {
    display: flex;
    align-items: center;
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


