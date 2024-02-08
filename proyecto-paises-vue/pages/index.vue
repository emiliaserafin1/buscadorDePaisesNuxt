<template>
  <div>
    <div class="filtros">
      <InputSearch @filtrarPorNombre="inputValue = $event"></InputSearch>
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
      listaDePaisesAux: [],
      inputValue: '',
      continente: 'default',
      ordenamiento: 'default',
      showModal: false,
      selectedCountry: {}
    }
  },

  computed: {
    paisesFiltradosArr() {
      let paisesFiltrados = this.listaDePaisesAux.slice();

      // Filtramos por continente si se selecciona uno
      if (this.continente !== 'default') {
        paisesFiltrados = paisesFiltrados.filter(pais => pais.region === this.continente);
      }

      // Aplicamos filtro por nombre si hay una búsqueda
      if (this.inputValue.trim() !== '') {
        const inputValueLowerCase = this.inputValue.toLowerCase();
        paisesFiltrados = paisesFiltrados.filter(pais => pais.name.common.toLowerCase().includes(inputValueLowerCase));
      }

      // Ordenamos según el criterio seleccionado
      if (this.ordenamiento === 'nombre-asc') {
        paisesFiltrados.sort((a, b) => a.name.common.localeCompare(b.name.common));
      } else if (this.ordenamiento === 'nombre-desc') {
        paisesFiltrados.sort((a, b) => b.name.common.localeCompare(a.name.common));
      } else if (this.ordenamiento === 'poblacion-asc') {
        paisesFiltrados.sort((a, b) => a.population - b.population);
      } else if (this.ordenamiento === 'poblacion-desc') {
        paisesFiltrados.sort((a, b) => b.population - a.population);
      }

      return paisesFiltrados;
    }
  },

  methods: {
    async obtenerPaises() {
      this.mostrarLoader();
      try {
        const response = await fetch(this.paisesApiUrl);
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

    mostrarModal(pais) {
      this.selectedCountry = pais;
      this.showModal = true;
    },

    cerrarModal() {
      this.showModal = false;
      this.selectedCountry = null;
    },

    filtrarPorNombre() {
      const inputValueLowerCase = this.inputValue.toLowerCase();
      const paisesEmpiezanCon = this.listaDePaisesAux.filter(pais => pais.name.common.toLowerCase().startsWith(inputValueLowerCase));
      const paisesContienen = this.listaDePaisesAux.filter(pais => pais.name.common.toLowerCase().includes(inputValueLowerCase) && !paisesEmpiezanCon.includes(pais));
      return paisesEmpiezanCon.concat(paisesContienen);
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

    limpiarFiltros(){
      this.obtenerPaises();
      this.inputValue = '';
      this.continente = 'default';
      this.ordenamiento = 'default';
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
        }
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


