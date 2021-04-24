<template>
<section>
<section v-if="!queen">
  <h3 id="welcome">Welcome</h3>
   <img id="rupaul-image" src="images/RuPaul.jpeg" alt="RuPaul">
</section>
<section v-if="queen">
  <h3 id="name">{{queen.name}}</h3>
  <div id="queen-tags">
    <p v-if="queen.winner" id="winner">Winner</p>
    <p v-if="queen.missCongeniality" id="miss-congeniality">Miss Congeniality</p>
  </div>
  <img :src="queen.image_url" :alt="queen.name" id="queen-image">
  <p v-if='queen.quote !=`""`' id="quote">{{queen.quote}}</p>
  <div id="seasons-stats">
  <div>
  <h4>Seasons</h4>
    <ul v-for="season in queen.seasonInfo.seasons" :key="season.queenId" id="seasons-list">
        <li>Season {{season.seasonNumber}} - {{season.place}}th Place</li>
    </ul>
  </div>
  <div>  
  <queen-stats :challengeWinData="challengeWinData"/>
  </div>  
  </div>
</section>
</section>
</template>

<script>
import QueenStats from './QueenStats'

export default {
    name: 'queen-detail',
    props: ['queen','challengeWinData'],
    data() {
      return {
        
      }
    },
    components: {
      'queen-stats': QueenStats
    }
}
</script>

<style scoped>
#queen-image {
    width: 250px;
    height: 300px;
    border-radius: 8px
}

#seasons-list {
    list-style: none;
    padding: 0px;
}
#queen-tags{
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  margin-top: 0;
  margin-bottom:10px;
}
#winner, #miss-congeniality {
  border: rgb(252, 85, 182) solid 4px;
  color: rgb(252, 85, 182);
  border-radius: 8px;
  padding: 5px;
  font-weight: bold;
  margin-top: 0;
}

#name {
  margin-top: 10px;
  margin-bottom: 10px;
}
#quote {
  border: #AEF3E7 solid 3px;
  font-weight: bold;
  width: 50%;
  margin-right: auto;
  margin-left: auto;
  background-color:#AEF3E7;
  border-radius: 8px;
}
#seasons-stats {
  display: grid;
  grid-template-columns: 1fr 1fr;

}
#welcome {
  font-size: 50px
}
</style>