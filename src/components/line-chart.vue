<template>
    <div class="chart-container">
        <canvas ref="LineChart" />
    </div>
</template>

<script>
    import Chart from "chart.js/auto";
    import "chartjs-adapter-moment";
    import "chartjs-plugin-gradient";
    import moment from "moment";

    export default {
        name: "Chart",
        props: {
            datasetNames: {
                type: Array,
                required: true,
                default: () => ["Series 3", "Series 2", "Series 1"],
            },
            datasetColors: {
                type: Array,
                required: true,
                default: () => [
                    "rgba(127, 86, 217, 1)",
                    "rgba(182, 146, 246, 1)",
                    "rgba(83, 56, 158, 1)",
                ],
            },
        },
        data() {
            return {
                myLineChart: null,
            };
        },
        mounted() {
            const datasets = this.generateRandomDatasets();
            const ctx = this.$refs.LineChart.getContext("2d");

            const bgColor1 = this.setAlphaChannel(datasets[0].color, 0.1);
            const bgColor2 = this.setAlphaChannel(datasets[0].color, 0);

            const gradientBg = ctx.createLinearGradient(0, 0, 0, 1400);

            gradientBg.addColorStop(0, bgColor1);
            gradientBg.addColorStop(0.1, bgColor2);

            this.createChart(ctx, datasets, gradientBg);
        },

        methods: {
            generateRandomDatasets() {
                return this.datasetNames.map((name, index) => ({
                    name,
                    color: this.datasetColors[index],
                    data: this.generateRandomData(),
                }));
            },
            generateRandomData() {
                const startDate = new Date("2023-01-01");
                const endDate = new Date("2023-12-31");
                const dateRange = this.getDateRange(startDate, endDate);

                let prevY = Math.floor(Math.random() * 200);
                const randomData = dateRange.map((date) => {
                    const yChange = this.getRandomArbitrary(-10, 10);
                    prevY += yChange;

                    return {
                        x: moment(date).format("YYYY-MM-DD"),
                        y: Math.max(0, Math.min(200, prevY)),
                    };
                });

                return randomData;
            },
            getDateRange(startDate, endDate) {
                const dateRange = [];
                let currentDate = new Date(startDate);

                while (currentDate <= endDate) {
                    dateRange.push(new Date(currentDate));
                    currentDate.setDate(currentDate.getDate() + 1);
                }

                return dateRange;
            },
            getRandomArbitrary(min, max) {
                return Math.random() * (max - min) + min;
            },

            setAlphaChannel(color, alpha) {
                const rgbValues = color.match(/\d+/g);
                const rgbaColor = `rgba(${rgbValues[0]}, ${rgbValues[1]}, ${rgbValues[2]}, ${alpha})`;
                return rgbaColor;
            },

            createChart(ctx, datasets, gradient) {
                this.myLineChart = new Chart(ctx, {
                    type: "line",
                    data: {
                        labels: datasets[0].data.map(
                            (dataPoint) => dataPoint.x
                        ),
                        datasets: datasets.map((data, index) => ({
                            label: data.name,
                            data: data.data.map((dataPoint) => dataPoint.y),
                            borderColor: data.color,
                            pointBackgroundColor: data.color,
                            backgroundColor: gradient,
                            borderWidth: 2,
                            width: 6,
                            fill: true,
                            tension: 0.6,
                        })),
                    },
                    options: {
                        maintainAspectRatio: false,
                        responsive: true,
                        scales: {
                            x: {
                                type: "time",
                                time: {
                                    unit: "day",
                                    displayFormats: {
                                        day: "MMM",
                                    },
                                },
                                ticks: {
                                    padding: 0,
                                    callback: (value) => {
                                        const currentDate = moment(value);
                                        return currentDate.date() === 15
                                            ? currentDate.format("MMM")
                                            : null;
                                    },
                                    font: {
                                        family: "Inter",
                                        weight: 400,
                                    },
                                },
                                grid: {
                                    display: false,
                                },
                                border: {
                                    display: false,
                                },
                                title: {
                                    display: true,
                                    text: "Month",
                                    font: {
                                        family: "Inter",
                                        weight: 500,
                                    },
                                    padding: 8,
                                },
                            },
                            y: {
                                display: true,
                                grid: {
                                    drawTicks: false,
                                },
                                ticks: {
                                    beginAtZero: true,
                                    padding: 6,
                                    font: {
                                        family: "Inter",
                                        weight: 400,
                                    },
                                },
                                border: {
                                    display: false,
                                },
                                title: {
                                    display: true,
                                    text: "Active Users",
                                    font: {
                                        family: "Inter",
                                        weight: 500,
                                    },
                                    padding: 16,
                                },
                            },
                        },
                        plugins: {
                            legend: {
                                display: true,
                                position: "right",
                                align: "start",
                                reverse: true,
                                labels: {
                                    usePointStyle: true,
                                    textAlign: "left",
                                    boxHeight: 3,
                                    boxWidth: 8,
                                    padding: 9,
                                },
                            },
                            tooltip: {
                                enabled: false,
                            },
                        },

                        elements: {
                            point: {
                                hoverRadius: 0,
                                pointRadius: 0,
                                pointStyle: "circle",
                            },
                        },
                    },
                });
            },
        },
    };
</script>

<style scoped>
    @import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;500&family=Red+Hat+Mono:wght@300;400;500;600;700&family=Space+Grotesk:wght@300;400;500;600;700&display=swap");
    .chart-container {
        max-width: 1232px;
        max-height: 240px;
        display: flex;
        width: 100%;
        height: auto;
        margin: 5px;
    }
    canvas {
        width: 100% !important;
        height: auto;
    }
</style>
