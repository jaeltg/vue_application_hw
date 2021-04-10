<template>
<section>
  <h3>{{queen.name}}</h3>
  <img :src="queen.image_url" :alt="queen.name" id="queen-image">
  <p v-if='queen.quote !=`""`'>{{queen.quote}}</p>
  <h4>Seasons:</h4>
    <ul v-for="season in queen.seasonInfo.seasons" :key="season.queenId" id="seasons-list">
        <li>Season {{season.seasonNumber}} - {{season.place}}th Place</li>
    </ul>
  <p v-if="queen.winner">Winner</p>
  <p v-if="queen.missCongeniality">Miss Congeniality</p>
</section>
</template>

<script>
export default {
    name: 'queen-detail',
    props: ['queen']
}
</script>

<style scoped>
#queen-image {
    width: 250px;
    height: 300px
}

#seasons-list {
    list-style: none;
}
</style>