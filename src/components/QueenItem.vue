<template>
  <li v-if="queen.name != 'Sh*rry P**'" @click="handleClick" id="queen-item">{{queen.name}}</li>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'queen-item',
    props: ['queen'],

    methods: {
        handleClick: function() {
            eventBus.$emit('queen-selected', this.queen)
        },

    }
}
</script>

<style scoped>
#queen-item:hover {
    background-color:rgb(252, 85, 182);
    color: white;

}

</style>