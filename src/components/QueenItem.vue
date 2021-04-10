<template>
  <li v-if="queen.name != 'Sh*rry P**'" @click="handleClick">{{queen.name}}</li>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'queen-item',
    props: ['queen'],

    data() {
        return {
            challengeWinData: [],
        }
    },

    methods: {
        handleClick: function() {
            eventBus.$emit('queen-selected', this.queen)
        },

    }
}
</script>

<style>

</style>