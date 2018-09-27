<template>
  <div id="app">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">PLAYER</h1>
        <div class="wins">
          <div class="wins text-center" :style="{width: userWins + '0%'}" style="background-color: green; margin: 0; color: white;"
               v-if="!showGame">
            {{userWins}}
          </div>
          <div else></div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">COMPUTER</h1>
        <div class="wins">
          <div class="wins text-center" :style="{width: compWins + '0%'}" style="background-color: green; margin: 0; color: white;"
               v-if="!showGame">
            {{compWins}}
          </div>
          <div else></div>
        </div>
      </div>
    </section>
    <section class="row controls" v-show="showGame">
      <div class="small-12 columns"  >
        <button id="start-game" @click= "startGame" >START NEW GAME</button>
      </div>
    </section>
    <section class="row controls" v-show="!showGame">
      <div class="small-12 columns"  >
        <button id="rock" @click="rockChoice">ROCK</button>
        <button id="paper" @click="paperChoice">PAPER</button>
        <button id="scissors" @click="scissorChoice">SCISSORS</button>
        <button id="restart" @click="restart">RESTART</button>
      </div>
    </section>
    <section class="row log" v-show="!showGame">
      <div class="small-12 columns" >
        <ul>
          <li v-for="result in resultLog" :class="{'player-turn': !result.compWin, 'computer-turn': result.compWin, 'tie-turn': result.tie}">
            {{result.text}}
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      compWins: 0,
      userWins: 0,
      highScore: 10,
      showGame: true,
      resultLog: [],
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {

    //start game
    startGame: function(){

      this.showGame = !this.showGame;
    },

    //restart game
    restart: function(){
      this.userWins = 0;
      this.compWins = 0;
      this.showGame = !this.showGame;
      this.resultLog = [];
    },

    rockChoice: function(){
      let computerChoice = this.compChoice();
      if (computerChoice === "paper") {
        this.resultLog.unshift({
          compWin: true,
          tie: false,
          text: "COMPUTER CHOSE PAPER | PAPER BEATS ROCK | COMPUTER WINS!"
        });
        this.compWins++;
      } else if (computerChoice === "scissors") {
        this.resultLog.unshift({
          compWin: false,
          tie: false,
          text: "COMPUTER CHOSE SCISSORS | ROCK BEATS SCISSORS | YOU WIN!"
        });
        this.userWins++;
      } else {
        this.resultLog.unshift({
          compWin: false,
          tie: true,
          text: "COMPUTER CHOSE ROCK | YOU'VE TIED!"
        });
      }
      this.gamesWinner();
    },

    paperChoice: function(){
      let computerChoice = this.compChoice();
      if (computerChoice === "scissors") {
        this.resultLog.unshift({
          compWin: true,
          tie: false,
          text: "COMPUTER CHOSE SCISSORS | SCISSORS BEATS PAPER | COMPUTER WINS!"
        });
        this.compWins++;
      } else if (computerChoice === "rock") {
        this.resultLog.unshift({
          compWin: false,
          tie: false,
          text: "COMPUTER CHOSE ROCK | PAPER BEATS ROCK | YOU WIN!"
        });
        this.userWins++;
      } else {
        this.resultLog.unshift({
          compWin: false,
          tie: true,
          text: "COMPUTER CHOSE PAPER | YOU'VE TIED!"
        });
      }
      this.gamesWinner();
    },

    scissorChoice: function(){
      let computerChoice = this.compChoice();
      if (computerChoice === "rock") {
        this.resultLog.unshift({
          compWin: true,
          tie: false,
          text: "COMPUTER CHOSE ROCK | ROCK BEATS SCISSORS | COMPUTER WINS!"
        });
        this.compWins++;
      } else if (computerChoice === "paper") {
        this.resultLog.unshift({
          compWin: false,
          tie: false,
          text: "COMPUTER CHOSE PAPER | SCISSORS BEATS PAPER | YOU WIN!"
        });
        this.userWins++;
      } else {
        this.resultLog.unshift({
          compWin: false,
          tie: true,
          text: "COMPUTER CHOSE SCISSORS | YOU'VE TIED!"
        });
      }
      this.gamesWinner();
    },

    //generate computer choice
    compChoice: function(){
      let computerChoice = Math.random();

      if(computerChoice < 0.34){
        return "rock";
      }else if(computerChoice > 0.34 && computerChoice < 0.67){
        return "paper";
      }else{
        return "scissors";
      }
    },

    //compare if comp won or user
    gamesWinner: function(){
      if(this.userWins === 10) {
        alert("You Won!");
        this.userWins = 0;
        this.compWins = 0;
        this.showGame = !this.showGame;
        this.resultLog = [];
      }
      else if(this.compWins === 10) {
        alert("Computer Wins! Sorry you Lose!");
        this.userWins = 0;
        this.compWins = 0;
        this.showGame = !this.showGame;
        this.resultLog = [];
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
