<template>
  <!-- Log for showing all the moves the player and monster make -->
  <div class="log" v-if="entries.length > 0">

    <!-- entries[] is an array that stores objects with two keys: turn and message -->
    <!-- turn stores who's turn it was that made the move, and is used to get the correct styling. -->
    <!-- message is what is actually displayed -->
    <ul v-for="entry in entries" :key="entry">
      <li :class="entry.turn">{{ entry.message }}</li>
    </ul>
  </div>
</template>


<script>
  export default {
    props: {
      entries: Array
    }
  }
</script>


<style scoped>
  .turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
  }

  .log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
  }

  .log ul li {
    margin: 5px;
  }

  .log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
  }

  .log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
  }
</style>
