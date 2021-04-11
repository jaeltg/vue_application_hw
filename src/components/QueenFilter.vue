<template>
<section id="filter-section">
  <h4>Filter By</h4>
  <div>
  <label for="queen-filter">Season:</label>   
  <select v-model="selectedSeason" @change="handleChange" name="queen-filter" id="queen-filter">
      <option value=" " selected disabled>Select a Season!</option>
      <option :value="null">All Seasons</option>
      <option v-for="season in seasons" :key="season.id" :value="season.id">
          Season {{season.seasonNumber}}
      </option>
  </select>
  </div>
  <div>
  <label for="queen-search">Name:</label>
  <input type="text" v-model="search" placeholder="Search for a Queen!" v-on:keyup="searchForQueen" id="queen-search">
  </div>
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

<style scoped>
#filter-section {
    text-align: center;
    margin-bottom: 10px;
    margin-left: 0px;
    background-color: rgb(252, 85, 182);
    color: white;
    padding-left: auto;
    padding-right: auto;
    padding-top: 10px;
    padding-bottom: 10px;
}
#filter-section > h4 {
    margin-top: 0px;
    color: white;
    margin-bottom: 10px
}

label {
    font-weight: bold;
    margin-right: 10px
}
</style>