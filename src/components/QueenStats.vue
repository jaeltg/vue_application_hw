<template>
<section>
  <h4>Challenges</h4>
  <chart type='PieChart' :data='challengeWinData' :options='chartOptions'/>
</section>
</template>

<script>
import {GChart} from 'vue-google-charts'
import {eventBus} from '../main.js'

export default {
name: 'queen-stats',
data () {
    return {
        chartOptions: {
            pieHole: 0.4,
            colors: ['#AEF3E7', 'rgb(252, 85, 182)'],
            fontName: 'Avenir',
            pieSliceText: 'none',
            legend: {
                position: 'labeled',
            },
            chartArea: {left:20,top:0,width:'85%',height:'80%'}
            
        }
    }
},
props: ['challengeWinData'],
components: {
    'chart': GChart
},

}
</script>

<style>

</style>