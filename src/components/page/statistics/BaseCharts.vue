<template>
    <div class="table">
        <div class="container">
            <table-list></table-list>
        </div>

    </div>
</template>

<script>
import TableList from "./components/BaseChartsList.vue";
    export default {
        name: 'basecharts',
        components: {
            TableList,
        },
        data: () => ({
        })
    }
</script>

<style scoped>
    
</style>