<template>
    <canvas ref="myCanvas"></canvas>
</template>

<script>
    export default {
        props: {
            circleColor: {
                type: String,
                default: "blue",
            },
        },
        mounted() {
            this.drawCircle();
        },
        methods: {
            drawCircle() {
                const canvas = this.$refs.myCanvas;
                const ctx = canvas.getContext("2d");

                // Retina ekran kontrolü
                const pixelRatio = window.devicePixelRatio || 1;

                // Canvas boyutları
                const canvasWidth = 8 * pixelRatio;
                const canvasHeight = 8 * pixelRatio;

                // Dairenin çapı
                const diameter = canvasWidth + 10;

                // Canvas boyutlarını belirle
                canvas.width = canvasWidth;
                canvas.height = canvasHeight;

                // Retina ekranları için scale faktörü
                ctx.scale(pixelRatio, pixelRatio);

                // Dairenin merkezi
                const centerX = canvasWidth / 2;
                const centerY = canvasHeight / 2;

                // Daireyi çiz
                ctx.beginPath();
                ctx.arc(centerX, centerY, diameter / 2, 0, 2 * Math.PI, false);

                // Dairenin kenar stilini belirle

                // Dairenin dolgu rengini belirle
                ctx.fillStyle = this.circleColor;

                // Daireyi doldur
                ctx.fill();
                ctx.closePath();
            },
        },
    };
</script>

<style scoped>
    /* Stil ekleyebilirsiniz, ancak daire zaten kod içinde çizildiği için bu örnekte gerek yok */
</style>
