<template>
    <div>
        <label for="pais-input">
            <input id="pais-input" type="text" placeholder="Ingrese un país" v-model="inputSearch" @keyup="search">
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
            },
            search() {
                setTimeout(() => {
                        this.$emit('filtrarPorNombre', this.inputSearch);
                }, 1000);
            }
        },
    }
</script>

<style scoped>
</style>
