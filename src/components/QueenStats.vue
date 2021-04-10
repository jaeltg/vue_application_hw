<template>
  <chart/>
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
props: ['winData'],
components: {
    'chart': GChart
},

}
</script>

<style>

</style>