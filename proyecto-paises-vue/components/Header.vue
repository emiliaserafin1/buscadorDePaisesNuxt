<template>
    <header>
        <h1>{{ title }}</h1>
    </header>
</template>

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

    i {
        margin: 0 40px 0 auto;
        font-size: 22px;
        cursor: pointer;
    }
</style>

 