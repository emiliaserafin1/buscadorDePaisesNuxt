<template>
    <div>
        <label for="pais-input">
            <input id="pais-input" type="text" placeholder="Ingrese un país" v-model="inputSearch" @input="filtrarPorNombre">
        </label>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                inputSearch: ''
            }
        },
        methods: {
            filtrarPorNombre() {
                this.$emit('filtrarPorNombre', this.inputSearch);
            }
        }
    }
</script>

<style scoped>
</style>
