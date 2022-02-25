<template>
  <div class="controls">

    <!-- New Game: When the user clicks new game you will load the battle menu buttons.  -->
    <div v-if="!playing">
      <button class="btn" id="start-game" @click="$emit('start-game')">START NEW GAME</button>
    </div>

    <div v-else>
      <button class="btn" id="attack" @click="$emit('attack')">ATTACK</button>
      <button class="btn" id="special-attack" @click="$emit('specialAttack')">SPECIAL ATTACK</button>
      <button class="btn" id="heal" @click="$emit('heal')">HEAL</button>
      <button class="btn" id="magicMissile" @click="$emit('magicMissile')">MAGIC MISSILE</button>

      <!-- Give Up: Giving up removes all of the battle option buttons and puts the start new game button 
          back in place.  -->
      <button class="btn" id="give-up" @click="$emit('giveUp')">GIVE UP</button>
    </div>

  </div>
</template>


<script>
  export default {
    name: 'Moves',
    props: {
      msg: String,
      playing: Boolean
    }
  }
</script>


<style scoped>
  button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
  }

  #start-game {
    background-color: #aaffb0;
  }

  #start-game:hover {
    background-color: #76ff7e;
  }

  #attack {
    background-color: #ff7367;
  }

  #attack:hover {
    background-color: #ff3f43;
  }

  #special-attack {
    background-color: #ffaf4f;
  }

  #special-attack:hover {
    background-color: #ff9a2b;
  }

  #heal {
    background-color: #aaffb0;
  }

  #heal:hover {
    background-color: #76ff7e;
  }

  #magicMissile {
    background-color: #aabeff;
  }

  #magicMissile:hover {
    background-color: #6383eb;
  }

  #give-up {
    background-color: #ffffff;
  }

  #give-up:hover {
    background-color: #c7c7c7;
  }
</style>
