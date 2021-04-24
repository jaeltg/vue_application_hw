<template>
  <section id="app">
  <img id="logo" src="../public/images/rupauls-logo.png" alt="RuPaul's Drag Race">
  <h1>Queens</h1>
  
  <div id="grid-container">
  <div>
  <queen-filter :seasons="seasons" :queens="queens"/>
  <queens-list :queens="filteredQueens"/>
  </div>  
  <queen-details :queen="selectedQueen" :challengeWinData="challengeWinData"/>
  </div>
  </section>
</template>

<script>
import {eventBus} from './main.js'

import QueensList from './components/QueensList'
import QueenDetails from './components/QueenDetails'
import QueenFilter from './components/QueenFilter'

export default {
  name: 'App',
  data() {
    return {
      queens: [],
      selectedQueen: null,
      seasons: [],
      selectedSeason: null,
      challengeWinData: [],
      selectedQueens: []
    }
  },
  computed: {
    filteredQueens: function() {
      let filteredArray = [];
      let queenArray = [];
      if (this.selectedQueens.length !== 0) {
        queenArray = this.selectedQueens
      } else {
        queenArray = this.queens
      }
      for (const queen of queenArray) {
        for (const season of queen.seasonInfo.seasons) {
          if (season.seasonId === this.selectedSeason) {
            filteredArray.push(queen)
          } else if (this.selectedSeason === null) {
           filteredArray = queenArray
          }
        }
      }
     return filteredArray
    }
  },

  watch: {
    selectedQueen: async function() {
      const response = await fetch(`http://www.nokeynoshade.party/api/queens/${this.selectedQueen.id}/challenges`)
      const data = await response.json()
      this.challengeWinData = this.formatChallengeWinData(data)
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

    eventBus.$on('season-selected', (seasonSent) => {
      this.selectedSeason = seasonSent
    })

    eventBus.$on('queens-selected', (queensSent) => {
      this.selectedQueens = queensSent
    })
    
  },

  components: {
    'queens-list': QueensList,
    'queen-details': QueenDetails,
    'queen-filter': QueenFilter
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
            seasonNumber: season.seasonNumber,
            seasonId: season.id
          }]
        }
        array.push(queenSeasonInfo)
       } 
      })
      return array
    },

    cleanSeasonInfoArray: function() {
      const seasonInfo = this.extractSeasonInfo()
      // obj - new object 
      // item - each season info object in the array
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
    },

    formatChallengeWinData: function(challengeData) {
      let array = [];
      let wins;
      let losses;
      const winData = challengeData.map((challenge) => {
        return challenge.won 
      })
      wins = winData.filter(Boolean).length/winData.length
      losses = 1 - wins
      array = [["Result", "Times"], ["Won", wins], ["Lost", losses]]
      return array 
    }

  }

  }

</script>

<style>
body {
  margin: 0%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

h1 {
  font-family: 'Lobster', cursive;
  font-size: 50px;
  color: rgb(252, 85, 182);
  margin-top: 10px;
  margin-bottom: 17px;
  width: 80%;
  border:  rgb(252, 85, 182) solid 4px;
  border-radius: 8px;
  margin-left: auto;
  margin-right: auto;
  background-color: #AEF3E7
}

h3 {
  font-family: 'Lobster', cursive;
  font-size: 30px;
  color: rgb(252, 85, 182)
}

h4{
  font-family: 'Lobster', cursive;
  font-size: 20px;
  color: rgb(252, 85, 182)
}

#grid-container {
  display: grid;
  grid-template-columns: 1fr 2fr
}

#logo {
  width: 100%;
  height: 150px;
  margin-bottom: 0;
}
</style>
