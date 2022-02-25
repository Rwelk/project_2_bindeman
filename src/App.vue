<template>
  <div>

    <!-- Display the health bars next to each other. -->
    <div class="row">
      <div class="col">  
        <HealthBar name="YOU" :hp="this.playerHealth"/>
      </div>
      <div class="col">
        <HealthBar name="MONSTER" :hp="this.monsterHealth"/>
      </div>
    </div>

    <!-- Display the various moves the player can make. -->
    <Moves :playing="this.playing"
      @startGame="startGame"
      @attack="playerTurn('attack')"
      @specialAttack="playerTurn('specialAttack')"
      @heal="playerTurn('heal')"
      @giveUp="playerTurn('giveUp')"
      @magicMissile="playerTurn('magicMissile')" />
    
    <!-- Display a log of the moves the player and monster have made. -->
    <BattleLog :entries="log"/>
  </div>
</template>


<script>
  import HealthBar from './components/HealthBar.vue'
  import Moves from './components/Moves.vue'
  import BattleLog from './components/BattleLog.vue'

  export default {
    name: 'App',
    components: {
      HealthBar, Moves, BattleLog
    },

    data() {
      return {
        playing: false,
        playerHealth: 100,
        monsterHealth: 100,
        log: []
      }
    },

    methods: {

      // New Game: When the user clicks new game you will reset the player and monster health bars and 
      //     clear out the actions list.
      startGame() {
        this.playing = true
        this.playerHealth = 100
        this.monsterHealth = 100
        this.log = []
      },


      // This method checks if the game is over.
      // Else if the monster wins, return 1.
      // Else the player wins so return 2.
      gameOver() {

        // If the game is still ongoing immediately return.
        if (this.playerHealth > 0 && this.monsterHealth > 0){
          return true
        }
        
        // Else someone won.
        let message = ""

        // If the monster won set the message to that.
        if (this.playerHealth <= 0) {
          this.playerHealth = 0
          message = "Monster wins."
        } 
        
        // Else the person won, so set the message to that instead.
        else {
          this.monsterHealth = 0
          message = "Player wins."
        }

        // Use an alert to tell the user the result and ask if they would like to start a new game. 
        // If they say yes reset everything for a new game. 
        if (confirm(message)) {
          this.startGame()
        }

        // If they say no leave the game as is, but remove the battle options and put start a new game 
        //     button in place. 
        else {
          this.playing = false
        }

        return false
      },
      

      // This method is a simple wrapper to get a random number between min and max.
      randomInterval(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min)
      },


      // The method triggers when the player clicks one of the buttons to make a move.
      // move is the action that will be performed.
      playerTurn(move) {
        let amount = 0
        let message = ""

        switch (move) {

          // Attack: When attacking the monster you do random damage between 3 and 10 and the log shows 
          //     the player hit the monster for X damage.
          default:
            amount = this.randomInterval(3, 10)
            message = `PLAYER HITS MONSTER FOR ${amount}`
            break

          // Special Attack: When special attacking the monster you do random damage between 10 and 20 and
          //     the log shows a the player hit the monster really hard for X damage. 
          case 'specialAttack':
            amount = this.randomInterval(10, 20)
            message = `PLAYER HITS MONSTER HARD FOR ${amount}`
            break

          // Heal: When healing you heal 10 health and the log shows the player heals for 10.
          case 'heal':
            if (this.playerHealth > 90) {
              amount = 100 - this.playerHealth
            } else{
              amount = 10
            }
            this.playerHealth += amount
            message = `PLAYER HEALS FOR ${amount}`
            break

          // Give Up: Giving up removes all of the battle option buttons and puts the start new game button 
          //     back in place.
          case 'giveUp':
            this.playing = false
            return

          // Custom Move: Create a custom move for your character to do.
          // The player has a 50/50 chance of hitting the monster for random damage between 20 and 30.
          case 'magicMissile':
            if (Math.random() > 0.5) {
              amount = this.randomInterval(20, 30)
              message = `PLAYER HITS MONSTER EXTRA HARD FOR ${amount}`
            }
            else {
              message = `PLAYER MISSES MONSTER FOR 0`
            }
            break
        }

        // The actual damage to the monster happens here.
        if (move != "heal") {
          this.monsterHealth -= amount
        }

        // After each action you should check to see if the player wins or loses.
        if (this.gameOver()) {
          
          // Add the message determined by what the player chose to do.
          // Use unshift() instead of append() to place the entry at the front of the list.
          this.log.unshift({
            turn: 'player-turn',
            message: message
          })

          // The monster moves after each player move (except for giving up).
          this.monsterTurn()

          // Once again check if the game is over in case the monster wins here.
          this.gameOver()
        }
      },


      // This method is for performing the monster's turn.
      monsterTurn() {

        // On the monster’s turn the monster does between 5 and 12 damage.
        const damage = this.randomInterval(5, 12)
        this.playerHealth -= damage
        
        // Add a message to the actions section showing the monster hit the player for X damage.
        this.log.unshift({
          turn: 'monster-turn',
          message: `MONSTER HITS PLAYER FOR ${damage}`

        })
      }
    }
  }
</script>


<style>
  .text-center {
    text-align: center;
  }

  .controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
    margin: 5%;
  }

  HealthBar {
    width: 50%;
  }
</style>
