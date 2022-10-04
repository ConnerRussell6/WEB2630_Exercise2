<template>
<div id="app">
  <section class="row">
    <div class="small-6 columns">
      <h1 class="text-center">PLAYER</h1>
      <div class="wins">
        <div class="wins text-center" style="background-color: green; margin: 0; color: white;"
        :style="{width: playerBar + '%'}">
          {{updatePlayerScore}}
          {{playerScore}}
        </div>
      </div>
    </div>
    <div class="small-6 columns">
      <h1 class="text-center">COMPUTER</h1>
      <div class="wins">
        <div class="wins text-center" style="background-color: green;  margin: 0; color: white;"
        :style="{width: computerBar + '%'}">
          {{updateComputerScore}}
          {{computerScore}}
        </div>
      </div>
    </div>
  </section>
  <section class="row controls" v-if="start">
    <div class="small-12 columns">
      <button id="start-game" @click="start=!start">START NEW GAME</button>
    </div>
  </section>
  <section class="row controls" v-else>
    <div class="small-12 columns">
        <button id="rock" @click="selectChoice(1)">ROCK</button>
        <button id="paper" @click="selectChoice(2)">PAPER</button>
        <button id="scissors" @click="selectChoice(3)">SCISSORS</button>
        <button id="restart" @click="reset()">RESTART</button>
    </div>
  </section>
  <section class="row log" v-if="!start">
    <div class="small-12 columns">
      <ul>
        <li v-for="item in output.slice().reverse()" :key="item.id" :class="{'player-turn': item.player, 'computer-turn': item.computer, 'tie-turn': item.tie}">
          {{item.message}}
          {{showResult}}
          {{endGame}}
        </li>
      </ul>
    </div>
  </section>
</div>
</template>

<script>
export default {
  data () {
    return {
      start: true, 
      computerChoice: -1,
      playerChoice: -1,
      computerScore: 0,
      playerScore: 0,
      output: [
        {message: ''},
        {player: false},
        {computer: false},
        {tie: false}
      ],
      playerBar: 0,
      computerBar: 0
    }
  },
  methods: {
    selectChoice (player) {
      this.playerChoice = player,
      this.computerChoice = Math.floor(Math.random() * 3 + 1)
    },

    reset () {
      this.playerScore = 0;
      this.computerScore = 0;
      this.playerChoice = -1;
      this.computerChoice = -1;
      this.playerBar = 0;
      this.computerBar = 0;
      this.start = true;
      this.output = [
        {message: ''},
        {player: false},
        {computer: false},
        {tie: false}
      ]
    },
    updatePlayerScore() {
      if(this.playerScore > 0) {
        this.playerBar = 10 * this.playerScore; 
      }
    },
    updateComputerScore() {
      if(this.computerScore > 0) {
        this.computerBar = 10 * this.computerScore; 
      }
    },
    showResult() {
      if (this.playerChoice == 1 && this.computerChoice == 3) {
        this.playerScore++
        this.output.push({message: 'COMPUTER CHOSE SCISSORS | ROCK BEATS SCISSORS | PLAYER WINS!', player: true})
      }
      else if (this.playerChoice == 1 && this.computerChoice == 2) {  
        this.computerScore++
        this.output.push({message: 'COMPUTER CHOSE PAPER | PAPER BEATS ROCK | COMPUTER WINS!', computer: true},)
      }
      else if (this.playerChoice == 1 && this.computerChoice == 1) {  
        this.output.push({message: 'COMPUTER CHOSE ROCK | YOU HAVE TIED!', tie: true})
      }
      else if (this.playerChoice == 2 && this.computerChoice == 1) {
        this.playerScore++
        this.output.push({message: 'COMPUTER CHOSE ROCK | PAPER BEATS ROCK | PLAYER WINS!', player: true})
      }
      else if (this.playerChoice == 2 && this.computerChoice == 3) {  
        this.computerScore++
        this.output.push({message: 'COMPUTER CHOSE SCISSORS | SCISSORS BEATS PAPER | COMPUTER WINS!', computer: true})
      }
      else if (this.playerChoice == 2 && this.computerChoice == 2) {  
        this.output.push({message: 'COMPUTER CHOSE PAPER | YOU HAVE TIED!',tie:true})
      }
      else if (this.playerChoice == 3 && this.computerChoice == 2) {
        this.playerScore++
        this.output.push({message: 'COMPUTER CHOSE PAPER | SCISSORS BEATS PAPER | PLAYER WINS!', player: true})
      }
      else if (this.playerChoice == 3 && this.computerChoice == 1) {  
        this.computerScore++
        this.output.push({message: 'COMPUTER CHOSE ROCK | ROCK BEATS SCISSORS | COMPUTER WINS!', computer: true})
      }
      else if (this.playerChoice == 3 && this.computerChoice == 3) {  
        this.output.push({message: 'COMPUTER CHOSE SCISSORS | YOU HAVE TIED!', tie: true})
      }
    },
    endGame() {
      if (this.playerScore == 10) {
        alert('Player Wins! Play Again?')
        this.reset()
      }
      else if (this.computerScore == 10) {
        alert('Computer Wins! Play Again?')
        this.reset()
      }
    }
  }
}
</script>

<style>
.text-center {
    text-align: center;
}

.wins {
    width: 80%;
    color: black;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 1000ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

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
    color: green;
    background-color: #aaffb0;
}

.log ul .computer-turn {
    color: red;
    background-color: #ffc0c1;
}

.log ul .tie-turn {
  color: blue;
  background-color: #e4e8ff;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #e4e8ff;
}

#start-game:hover {
  background-color: #687eff;
}

#rock {
    background-color: #ff7367;
}

#rock:hover {
    background-color: #ff3f43;
}

#paper {
    background-color: #ffaf4f;
}

#paper:hover {
    background-color: #ff9a2b;
}

#scissors {
    background-color: #aaffb0;
}

#scissors:hover {
    background-color: #76ff7e;
}

#restart {
    background-color: #ffffff;
}

#restart:hover {
    background-color: #c7c7c7;
}
</style>
