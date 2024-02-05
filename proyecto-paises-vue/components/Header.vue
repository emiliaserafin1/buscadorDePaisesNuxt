<script>
    export default {
        props: {
            title: {
                type: String,
                required: true
            }
        }
    }
</script>

<template>
    <header>
        <h1>{{ title }}</h1>
    </header>
</template>

<style scoped>
    header {
        height: 70px;
        display: flex;
        align-items: center;
        border-bottom: 1px solid #464646;
    }

    header h1 {
        margin-left: 40px;
        font-size: 28px;
        font-weight: 500;
    }
</style>
 