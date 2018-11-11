<template>
  <div>
    <div class="gameStatus" :class="gameStatusColor">
      {{ gameStatusMessage }}
    </div>
    <table class="grid">
      <tr>
        <cell name="1" :mark="this.cells[1]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="2" :mark="this.cells[2]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="3" :mark="this.cells[3]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
      </tr>  
      <tr>
        <cell name="4" :mark="this.cells[4]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="5" :mark="this.cells[5]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="6" :mark="this.cells[6]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
      </tr>  
      <tr>
        <cell name="7" :mark="this.cells[7]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="8" :mark="this.cells[8]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
        <cell name="9" :mark="this.cells[9]" v-on:strike="update" :gameStatus="gameStatus"></cell>  
      </tr>  
    </table>
  </div>
</template>

<script>
import Cell from './Cell';
export default {
  props: ["restartGame"],
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
    gameStatus: {
      get: function () {
        if (this.moves === 9) {
          return 'draw';
        } else if (this.checkForWin) {
          return 'win';
        } else {
          return "turn";
        }
      },
      set: function (val) {
        return val;
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
        this.$emit('gameOver', { winningPlayer: this.activePlayer });
        return;
      } else if (this.gameStatus === 'draw') {
        this.gameStatusColor = 'statusDraw';
        this.gameStatusMessage = `It's a draw!`;
        this.$emit('gameOver', {
          condition: 'draw',
        });
        return;
      }
        this.gameStatusMessage = `${this.changePlayer(this.activePlayer)}'s turn` 
        return;
    },
  },
  watch: {
    restartGame() {
      if (this.restartGame) {
        this.gameStatus = 'turn';
        this.gameStatusColor = 'statusTurn';
        this.activePlayer = 'O';
        this.gameStatusMessage = "O's turn";
        this.moves = 0;
        for (let key of Object.keys(this.cells)) {
          this.cells[key] = '';
        }
        this.$emit('init');
      }
    }
  }
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
