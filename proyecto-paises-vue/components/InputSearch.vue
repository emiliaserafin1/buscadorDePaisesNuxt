<template>
    <div class="filtros">
        <label for="pais-input">
            <input id="pais-input" type="text" placeholder="Ingrese un país" v-model="inputValue" @input="filtrarPorNombre">
        </label>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                inputValue: ''
            }
        },
        methods: {
            filtrarPorNombre() {
                console.log('filtrarPorNombre', this.inputValue)
                this.$emit('filtrarPorNombre', this.inputValue);
            }
        }
    }
</script>

<style scoped>
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

</style>
