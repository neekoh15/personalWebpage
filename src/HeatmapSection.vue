<template>
    <h2>About me</h2>

    <div class="heatmap-wrapper">
        <div id="heatmap">
            <div>

            </div>
        </div>
    </div>
    
</template>

<script>
import Heatmap from 'heatmap.js'
import { ref, onMounted } from 'vue';

export default {
    name: 'HeatmapSection',
    setup() {
        const heatmapPoints = ref([]);
        const heatmapInstance = ref()

        const generateHeatmap = () => {
            var heatmapInstance = Heatmap.create({
                container: document.querySelector('#heatmap'),
                radius: 20
            });

            document.querySelector('#heatmap').onmousemove = function(ev) {
                heatmapInstance.addData({
                    x: ev.layerX,
                    y: ev.layerY,
                    value: 1
                });
            }
        };

        onMounted(() => {
            heatmapInstance.value = Heatmap.create({
                container: document.querySelector('#heatmap'),
                radius: 70
            });

            document.querySelector('.heatmap-wrapper').onmousemove = function(ev) {
                const point = {
                    x: ev.layerX,
                    y: ev.layerY,
                    value: 3
                };

                if (heatmapPoints.value.length > 50) {
                    heatmapPoints.value.shift();
                }
                heatmapPoints.value.push(point);
                
                heatmapInstance.value.setData({ max: 5, data: heatmapPoints.value });
            };
        });

        return {
            heatmapPoints,
            generateHeatmap
        };
    }
}
</script>

<style scoped>

.heatmap-wrapper {
}

#heatmap {
    box-sizing: border-box;
    width: 100%;
    height: 40rem;
    padding-bottom: 3rem;
    border: 1px solid #ccc;
}
</style>