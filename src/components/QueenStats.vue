<template>
<section>
  <h4>Challenges</h4>
  <chart type='PieChart' :data='challengeWinData' :option='chartOptions'/>
</section>
</template>

<script>
import {GChart} from 'vue-google-charts'
import {eventBus} from '../main.js'

export default {
name: 'queen-stats',
data () {
    return {
        chartOptions: {}
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