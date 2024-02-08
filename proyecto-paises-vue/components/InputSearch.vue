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
                inputSearch: '',
                timer: null,
                lastValueEmitted: ''
            }
        },
        methods: {
            search() {
            clearTimeout(this.timer);
            this.timer = setTimeout(() => {
                if (this.inputSearch !== this.lastValueEmitted) {
                    this.lastValueEmitted = this.inputSearch;
                    this.$emit('filtrarPorNombre', this.inputSearch);
                }
            }, 1000);
        }
        },
    }
</script>

<style scoped>
</style>
