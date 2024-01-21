<template>
    <div id="index">
        <Chart />
    </div>
</template>

<script>
    import Chart from "./components/line-chart.vue";
    export default {
        name: "App",
        components: {Chart},
    };
</script>

<style>
    * {
        margin: 0;
    }

    #index {
        height: 100%;
        width: 100%;
        display: flex;
        justify-content: center;
        position: fixed;
        align-items: center;
    }
</style>
