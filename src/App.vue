<template>
    <div id="index">
        <Chart />
        <!-- <Circle /> -->
    </div>
</template>

<script>
    import Chart from "./components/chart.vue";
    import Circle from "./components/circle.vue";
    export default {
        name: "App",
        components: {Chart, Circle},
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
