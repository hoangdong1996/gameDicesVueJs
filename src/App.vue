<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:currentScore="currentScore"
        v-bind:activePlayer="activePlayer"
      ></players>
      <controls
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:handleHold="handleHold"
        v-on:handleFinalScore="handleFinalScore"
        v-bind:finalScore="finalScore"
        v-bind:isPlaying="isPlaying"
      ></controls>
      <dices v-bind:dices="dices"></dices>
      <popupRule v-bind:isOpenPopup="isOpenPopup" v-on:handleConfirm="handleConfirm"></popupRule>
    </div>
  </div>
</template>

<script>
import Players from "./components/Players";
import Controls from "./components/Controls";
import Dices from "./components/Dices";
import PopupRule from "./components/PopupRule";

export default {
  name: "App",
  data() {
    return {
      isPlaying: false,
      isOpenPopup: false,
      activePlayer: 0, //Người chơi hiện tại
      scoresPlayer: [0, 0],
      currentScore: 0,
      dices: [1, 1],
      finalScore: 0,
    };
  },
  computed: {
    isWinner() {
      let { scoresPlayer, finalScore } = this;
      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
        this.isPlaying = false;
        return true;
      }
      return false;
    },
  },
  methods: {
    handleNewGame() {
      this.isOpenPopup = true;
      console.log("new game in app");
    },
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scoresPlayer = [0, 0];
      this.currentScore = 0;
      console.log("confirm trong app");
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleRollDice() {
      console.log("roll trong app");
      if (this.isPlaying) {
        var dice1 = Math.floor(Math.random() * 6 + 1);
        var dice2 = Math.floor(Math.random() * 6 + 1);
        console.log(dice1, dice2);

        this.dices = [dice1, dice2];
        
        if (dice1 === 1 || dice2 === 1) {
            // let player = this.activePlayer;
          setTimeout(() => {
            alert(
              "Người chơi Player " +
                $(this.activePlayer + 1) +
                " đã quay chúng xuc xắc 1 "
            );
          }, 10);
          this.nextPlayer();
        } else {
          this.currentScore = this.currentScore + dice1 + dice2;
        }
      } else {
        alert("Vui lòng nhấn new game");
      }
    },
    handleHold() {
      if (this.isPlaying) {
        if (this.activePlayer === 0) {
          this.scoresPlayer[0] += this.currentScore;
        } else {
          this.scoresPlayer[1] += this.currentScore;
        }
        if (!this.isWinner) {
          this.nextPlayer();
        }
      } else{
      alert("Vui lòng nhấn new game");
      }
      console.log("hold trong app");
    },
    handleFinalScore(event) {
      var number = parseInt(event.target.value);
      if (isNaN(number)) {
        this.finalScore = "";
      } else {
        this.finalScore = number;
      }
      console.log(parseInt(event.target.value));
    },
  },
  components: {
    Players,
    Controls,
    Dices,
    PopupRule,
  },
};
</script>

<style>
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
    url(./assets/back.jpg);
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
</style>
