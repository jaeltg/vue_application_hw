<template>
  <section id="queens-scroll">
      <ul id="queens-list">
          <queen-item v-for="queen in queens" :key="queen.id" :queen="queen" />
      </ul>
  </section>
</template>

<script>
import QueenItem from './QueenItem'

export default {
  name: 'queens-list',
  props: ['queens'],
  components: {
      'queen-item': QueenItem
  } 
}
</script>

<style>
#queens-scroll {
    overflow-y: scroll;
    height: 400px;
    width: auto;
}


#queens-list {
    list-style: none;
    text-align: center;
}
</style>