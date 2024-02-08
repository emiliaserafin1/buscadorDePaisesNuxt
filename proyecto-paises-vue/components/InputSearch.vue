<template>
    <div>
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
                this.$emit('filtrarPorNombre', this.inputValue);
            }
        }
    }
</script>

<style scoped>
</style>
