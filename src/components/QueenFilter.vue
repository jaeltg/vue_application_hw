<template>
  <select v-model="selectedSeason" @change="handleChange" name="queen-filter" id="queen-filter">
      <option v-for="season in seasons" :key="season.id" :value="season.id">
          Season {{season.seasonNumber}}
      </option>
  </select>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'queen-filter',

    props: ['seasons'],

    data() {
        return {
            selectedSeason: {}
        }
    },

    methods: {
        handleChange: function() {
            eventBus.$emit('season-selected', this.selectedSeason)
        }
    }
}
</script>

<style>

</style>