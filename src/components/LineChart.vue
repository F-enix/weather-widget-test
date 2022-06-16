<template>
    <div class="flex-box">
        <canvas id="myChart" width="650" height="400"></canvas>
    </div>
</template>

<script>
import Chart from 'chart.js/auto';

export default {
    name: 'LineChart',
    data() {
        return {
            arrTemp: [],
            arrDays: []
        }
    },
    props: ['forecast', 'dateBuilder'],
    methods: {
        fillData(){
            for(let i=0;i<8;i++){
                this.arrTemp[i] = Math.round(this.forecast.daily[i].temp.max);
                this.arrDays[i] = this.dateBuilder(i,true);
            }
        }
    },
    computed: {
        
    },
    created() {
        this.fillData();
    },
    mounted(){
        
        const ctx = document.getElementById("myChart").getContext("2d");
        let gradient = ctx.createLinearGradient(0,0,0,300);
        gradient.addColorStop(0, 'rgba(255,30,0,1)');
        gradient.addColorStop(1, 'rgba(180,80,40,0.15)');

        const myChart = new Chart(ctx, {
        type: 'line',
        data: {
            labels: this.arrDays,
            datasets: [{
                label: 'Weekly max temperature\'s',
                data: this.arrTemp,
                fill: true,
                borderColor: 'rgb(75, 192, 192)',
                backgroundColor: gradient,
                tension: 0.2
            }]
        },
        options: {
            scales: {
                y: {
                    type: 'linear',
                    grace: '70%'
                }
            }
        }
        });
        myChart;
    }
}
</script>

<style scoped>
.flex-box {
    min-width: 250px;
    width: 100%;
    max-width: 800px;
}
</style>