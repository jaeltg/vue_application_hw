<template>
<section>
  <select v-model="selectedSeason" @change="handleChange" name="queen-filter" id="queen-filter">
      <option selected disabled="disabled">Select a Season!</option>
      <option :value="null">All Seasons</option>
      <option v-for="season in seasons" :key="season.id" :value="season.id">
          Season {{season.seasonNumber}}
      </option>
  </select>
  <input type="text" v-model="search" placeholder="search for queen..." v-on:keyup="searchForQueen">
</section> 
</template>

<script>
import {eventBus} from '../main.js'

export default {
    name: 'queen-filter',

    props: ['seasons', 'queens'],

    data() {
        return {
            selectedSeason: {},
            search: '',
            selectedQueens: {},
        }
    },

    methods: {
        handleChange: function() {
            eventBus.$emit('season-selected', this.selectedSeason)
        },
        searchForQueen: function() {
            let foundQueens = this.queens.filter((queen) => {
               return queen.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1
            })
            this.selectedQueens = foundQueens

            eventBus.$emit('queens-selected', this.selectedQueens)
        }
    }
}
</script>

<style>

</style>