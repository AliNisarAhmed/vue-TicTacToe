<template>
  <div>
    <div class="gameStatus" :class="gameStatusColor">
      {{ gameStatusMessage }}
    </div>
    <table class="grid">
      <tr>
        <cell name="1" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="3" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="2" v-on:strike="update" :gameStatus="gameStatus"></cell>  
      </tr>  
      <tr>
        <cell name="4" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="5" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="6" v-on:strike="update" :gameStatus="gameStatus"></cell>  
      </tr>  
      <tr>
        <cell name="7" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="8" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="9" v-on:strike="update" :gameStatus="gameStatus"></cell>  
      </tr>  
    </table>
  </div>
</template>

<script>
import Cell from './Cell';
export default {
  components: {
    'cell': Cell
  },
  data () { 
    return {
      activePlayer: 'O',
      gameStatusMessage: "O's turn", // "X's turn"
      gameStatusColor: 'statusTurn', // statusWin, statusDraw
      moves: 0, // max = 9
      cells: {
        1: '', 2: '', 3: '',
        4: '', 5: '', 6: '',
        7: '', 8: '', 9: ''
      },
      winConditions: [
        [1, 2, 3], [4, 5, 6], [7, 8, 9],
        [1, 4, 7], [2, 5, 8], [3, 6, 9],
        [1, 5, 9], [3, 5, 7]
      ]
    };
  },
  computed: {
    gameStatus () {
      if (this.checkForWin) {
        return 'win';
      } else if (this.moves === 9) {
        return 'draw';
      } else {
        return "turn";
      }
    },
    checkForWin () {
      let cells = this.cells;
      for (let wc of this.winConditions) {
        let [ first, second, third ] = wc;
        if (cells[first] && cells[second] && cells[third] && cells[first] === cells[second] && cells[second] === cells[third]) {
          return true;
        }
      }
      return false;
    }
  },
  methods: {
    update (name) {
      this.cells[name] = this.activePlayer; 
      this.moves++;
      this.changeGameStatus();
      this.activePlayer = this.changePlayer(this.activePlayer);
    },
    changePlayer(activePlayer) {
      return activePlayer === "O" ? "X": 'O';
    },
    changeGameStatus () {
      if (this.gameStatus === 'win') {
        this.gameStatusColor = 'statusWin';
        this.gameStatusMessage = `${this.activePlayer} wins!`;
        return;
      } else if (this.gameStatus === 'draw') {
        this.gameStatusColor = 'statusDraw';
        this.gameStatusMessage = `It's a draw!`;
        return;
      }
        this.gameStatusMessage = `${this.changePlayer(this.activePlayer)}'s turn` 
        return;
    },
  },
  
}
</script>

<style>
.grid {
  background-color: #34495e;
  color: #fff;
  width: 100%;
  border-collapse: collapse;
}

.gameStatus {
  margin: 0px;
  padding: 15px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  background-color: #f1c40f;
  color: #fff;    
  font-size: 1.4em;
  font-weight: bold;
}

.statusTurn {
    background-color: #f1c40f;
}

.statusWin {
    background-color: #2ecc71;
}

.statusDraw {
    background-color: #9b59b6;
}
</style>
