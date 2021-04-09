<template>
  <section id="app">
  <h1>RuPaul's Drag Race Queens!!!</h1>
  <div id="grid-container">
  <queens-list :queens="queens"/>
  <queen-details v-if="selectedQueen" :queen="selectedQueen" :seasons="seasons"/>
  </div>
  </section>
</template>

<script>
import {eventBus} from './main.js'

import QueensList from './components/QueensList'
import QueenDetails from './components/QueenDetails'

export default {
  name: 'App',
  data() {
    return {
      queens: [],
      selectedQueen: null,
      seasons: [],
    }
  },

  async mounted(){
    const response1 = await fetch('http://www.nokeynoshade.party/api/seasons')
    const data1 = await response1.json()
    this.seasons = data1
    

    const response = await fetch('http://www.nokeynoshade.party/api/queens/all')
    const data = await response.json()
    this.queens = this.addSeasonInfoToQueen(data)
  

    eventBus.$on('queen-selected', (queenSent) => {
      this.selectedQueen = queenSent;
    })
  },

  components: {
    'queens-list': QueensList,
    'queen-details': QueenDetails
  },

  methods: {
    extractSeasonInfo: function () {
    let array = []
    let queenSeasonInfo = {}
    this.seasons.forEach((season) => { 
      for (const queen of season.queens) {
        queenSeasonInfo = {
          queenId: queen.id,
          seasons: [{
            place: queen.place,
            season: season.seasonNumber,
            seasonID: season.id
          }]
        }
        array.push(queenSeasonInfo)
       } 
      })
      return array
    },

    cleanSeasonInfoArray: function() {
      const seasonInfo = this.extractSeasonInfo()
      const cleanedObject = seasonInfo.reduce((obj, item) => {
        obj[item.queenId] ? obj[item.queenId].seasons.push(...item.seasons) : (obj[item.queenId] ={...item});
        return obj;
      }, {});
      const cleanedArray = Object.values(cleanedObject)
      return cleanedArray
    },

    addSeasonInfoToQueen: function(queens) {
      const cleanedSeasonInfo = this.cleanSeasonInfoArray()
      queens.map((queen) => {
        for (const info of cleanedSeasonInfo) {
          if (info.queenId === queen.id) {
            queen["seasonInfo"] = info
          }
        }
      })
      return queens
    }

  }

  }

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#grid-container {
  display: grid;
  grid-template-columns: 1fr 3fr
}
</style>
