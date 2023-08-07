<template>
  <div id="app">
    <div class="wrapper clearfix">
      <player 
        v-bind:isWinner="isWinner"
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:currentScore="currentScore"
        v-bind:activePlayer="activePlayer"       
      />
      <control 
        v-bind:isPlay="isPlay"
        v-bind:finalScore="finalScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        v-on:newGame="handleNewGame"
        v-on:newRollDice="handleRollDice"
        v-on:handleHold="handleHold"
      />
      <dices 
        v-bind:dices="dices"
      />
      <poppup-rule
        v-bind:isOpenPopup="isOpenPopup"
        v-on:handleConfirm="handleConfirm"
      />
    </div>
  </div>
</template>

<script>
import Player from "./components/Player.vue";
import Control from "./components/Control.vue";
import Dices from "./components/Dices.vue";
import PoppupRule from "./components/PoppupRule.vue";
export default {
  components: { Player, Control, Dices, PoppupRule },
  name: "app",
  data() {
    return {
        scoresPlayer:[13,30],
        currentScore:0,
        activePlayer:1 ,//nhan dien ai la nguoi choi hien tai
        isOpenPopup:false,
        isPlay:false,
        dices:[1,5],
        finalScore:10
    };
  },
  comments: {
    Player,
    Control,
    Dices,
    PoppupRule
  },
  methods:{
    handleChangeFinalScore(e){
      console.log(e.target)
    },
    handleNewGame(e){       
        this.isOpenPopup = true;              
    },
    handleConfirm(e){
      this.isOpenPopup = false;
      this.isPlay = true;
      this.activePlayer = 0;
      this.scoresPlayer=[0,0];
      this.currentScore = 0;
      this.dices=[1,1];
    },
    nextPlayer(){
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleRollDice(e){
      if(this.isPlay){
        var dice1=Math.floor(Math.random() * 6 ) +1;
        var dice2=Math.floor(Math.random() * 6 ) +1;
        this.dices=[dice1,dice2];
        if(dice1 === 1 || dice2 === 1){
          let nextActive = this.activePlayer;
          setTimeout(function() {
            alert(`Người chơi ${nextActive+1} đã quay trúng số 1`)           
          },5)
          
          this.nextPlayer();
        }else{
          this.currentScore = this.currentScore + dice1 + dice2;
        }
      }else{
        alert("Vui lòng ấn nút new game để chơi")
      }
    },
    handleHold(){
     if(this.isPlay){
      let {scoresPlayer,activePlayer,currentScore} = this;
      let scoreOld = scoresPlayer[activePlayer];
      this.$set(this.scoresPlayer,activePlayer,scoreOld +currentScore);
      if(!this.isWinner){
        this.nextPlayer();
      }
     }else{
      alert("Vui lòng ấn New Game để bắt đầu chơi")
     }
    }
  },
  computed:{
    isWinner(){
      let {scoresPlayer,finalScore} = this;
      if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore){
        this.isPlay=false     
        return true;
      }
      return false;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url(../public/assets/back.png);
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}

#dice-1 {
  top: 120px;
}
#dice-2 {
  top: 250px;
}

.dice {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 120px;
  height: 120px;
  box-shadow: 0px 10px 60px rgba(0, 0, 0, 0.1);
}
.spinner div {
  position: absolute;
  width: 120px;
  height: 120px;
  border: 1px solid #ccc;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  text-align: center;
  line-height: 120px;
  font-size: 100px;
  color: #42b983;
  font-size: 0;
  transition: all 0.3s ease;
  opacity: 1;
}

.spinner .face1 {
  -webkit-transform: translateZ(60px);
  -ms-transform: translateZ(60px);
  transform: translateZ(60px);
  background-image: url("../public/assets/dice-1.png");
  background-position: center;
  background-size: cover;
}
.spinner .face2 {
  -webkit-transform: rotateY(90deg) translateZ(60px);
  -ms-transform: rotateY(90deg) translateZ(60px);
  transform: rotateY(90deg) translateZ(60px);
  background-image: url("../public/assets/dice-2.png");
  background-position: center;
  background-size: cover;
}
.spinner .face3 {
  -webkit-transform: rotateY(90deg) rotateX(90deg) translateZ(60px);
  -ms-transform: rotateY(90deg) rotateX(90deg) translateZ(60px);
  transform: rotateY(90deg) rotateX(90deg) translateZ(60px);
  background-image: url("../public/assets/dice-3.png");
  background-position: center;
  background-size: cover;
}
.spinner .face4 {
  -webkit-transform: rotateY(180deg) rotateZ(90deg) translateZ(60px);
  -ms-transform: rotateY(180deg) rotateZ(90deg) translateZ(60px);
  transform: rotateY(180deg) rotateZ(90deg) translateZ(60px);
  background-image: url("../public/assets/dice-4.png");
  background-position: center;
  background-size: cover;
}
.spinner .face5 {
  -webkit-transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px);
  -ms-transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px);
  transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px);
  background-image: url("../public/assets/dice-5.png");
  background-position: center;
  background-size: cover;
}
.spinner .face6 {
  -webkit-transform: rotateX(-90deg) translateZ(60px);
  -ms-transform: rotateX(-90deg) translateZ(60px);
  transform: rotateX(-90deg) translateZ(60px);
  background-image: url("../public/assets/dice-6.png");
  background-position: center;
  background-size: cover;
}

.spinner {
  -webkit-transform-style: preserve-3d;
  -ms-transform-style: preserve-3d;
  transform-style: preserve-3d;
  -webkit-transform-origin: 60px 60px 0;
  -ms-transform-origin: 60px 60px 0;
  transform-origin: 60px 60px 0;
  -webkit-transition: all 0.9s ease;
  -o-transition: all 0.9s ease;
  transition: all 0.9s ease;
  -webkit-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  -ms-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  -o-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
}
.spinner.dice-1 {
  -webkit-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  -ms-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  -o-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
}
.spinner.dice-2 {
  -webkit-transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg);
  -ms-transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg);
  -o-transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg);
  transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg);
}
.spinner.dice-3 {
  -webkit-transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg);
  -ms-transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg);
  -o-transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg);
  transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg);
}
.spinner.dice-4 {
  -webkit-transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg);
  -ms-transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg);
  -o-transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg);
  transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg);
}
.spinner.dice-5 {
  -webkit-transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg);
  -ms-transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg);
  -o-transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg);
  transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg);
}
.spinner.dice-6 {
  -webkit-transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg);
  -ms-transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg);
  -o-transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg);
  transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg);
}
</style>
