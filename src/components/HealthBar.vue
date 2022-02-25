<template>
  <div class="healthBar">

    <!-- Display who's health it is -->
    <h1 class="text-center mt-3">{{ name }}</h1>

    <!-- There are two parts of the health bar, a grey background, and a green section that shows how much
      health remains. -->
    <!-- The latter is nested inside the former so they will overlap. -->
    <div class="healthbar text-center">

      <!-- cssVars is a computed method that returns the percentage of the health bar that should be 
        "filled" in. -->
      <div class="healthbar foreground" :style="cssVars">
        
        <!-- Also display a numerical value for how much health remains. -->
        <p>{{ hp }}</p>
      </div>
    </div>
  </div>
</template>


<script>
  export default {
    name: 'HelloWorld',
    props: {
      name: String,
      hp: Number
    },

    data() {
      return {
      }
    },
    
    computed: {
      cssVars () {
        return {
          '--hp': this.hp + "%"
        }
      }
    }
  }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .healthbar {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
  }
  .foreground {
    width: var(--hp);
    background-color: green;
    margin: 0;
    color: white;
  }
</style>
