<template>
  <div id="app">
    <div id="details">
      <h1>Tic Tac Toe</h1>
    </div>
    <div class="scoreBoard">
      <span>O has {{ wins.O }} wins</span>
      <h2>Score Board</h2>
      <span>X has {{ wins.X }} wins</span>
    </div>
    <Grid @gameOver="gameOver" :restartGame="restartGame" @init="init"></Grid>
    <button class="restart" v-if="restartVisible" @click="restart">Restart</button>
  </div>
</template>

<script>
import Grid from './components/Grid.vue'

export default {
  name: 'app',
  components: {
    Grid
  },
  data () {
    return {
      matches: 0,
      wins: {
        O: 0,
        X: 0
      },
      restartVisible: false,
      restartGame: false
    };
  },
  methods: {
    gameOver (payload) {
      if (payload.winningPlayer) {
        payload.winningPlayer === 'O'? this.wins.O++: this.wins.X++;
      }
      this.matches++;
      this.restartVisible = true;
      return; 
    },
    restart () {
      this.restartVisible = false;
      this.restartGame = true;
    },
    init () {
      this.restartGame = false;
    }
  }
}
</script>

<style>
body {
  background-color: #fff;
  color: #fff;
  font-family: 'Dosis', Helvetica, sans-serif;
  text-align: center;
  margin: 0;
}

#app {
  margin: 0 auto;
  max-width: 310px;
  color: #34495e;
  display: flex;
  flex-flow: column;
  justify-content: center;
}

h1 {
  text-transform: uppercase;
  font-weight: bold;
  font-size: 3em;
}

.restart {
  background-color: #e74c3c;
  color: #fff;
  border: 0px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  font-family: 'Dosis', Helvetica, sans-serif;
  font-size: 1.4em;
  font-weight: bold;
  margin: 0px;
  padding: 15px;
  width: 100%;
}

.restart:hover {
  background-color: #c0392b;
  cursor: pointer;
}

.scoreBoard {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  height: 15px;
  background-color: #16a085;
  box-shadow: 10px solid #fff;
  border-radius: 10px;
  font-size: 14px;
  padding: 10px;
  margin-bottom: 15px;
}

.scoreBoard h2 {
  margin: 0px;
}

.scoreBoard span {
  font-weight: bold;
}
</style>
