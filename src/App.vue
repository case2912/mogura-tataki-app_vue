<template>
  <v-app>
    <v-content class="app-container">
      <v-btn large round @click="startGame">game start</v-btn>
      <div class="counter-container">
        <Counter label="Score" :value="score"/>
        <Counter label="High Score" :value="highScore"/>
        <Counter label="Accuracy" :value="hitPercentage"/>
        <Counter label="Timer" :value="timer"/>
      </div>
      <Moles :moleData="moles" v-on:whack="handleMoleWhack" v-on:miss="handleMoleMiss"/>
    </v-content>
  </v-app>
</template>

<script>
import Counter from "./components/Counter";
import Moles from "./components/Moles";
export default {
  name: "App",
  components: {
    Counter,
    Moles
  },
  data: function() {
    return {
      score: 0,
      highScore: 0,
      timer: 20,
      gameTime: 20,
      startGameTime: 0,
      totalClickCount: 0,
      interval: 3,
      moles: [false, false, false, false],
      gameActive: false
    };
  },
  methods: {
    startGame: function() {
      if (this.gameActive) return;
      this.gameActive = true;
      this.startGameTime = performance.now();
      this.activateRandomMole();
      this.gameLoop(performance.now());
    },
    endGame: function() {
      this.timer = this.gameTime;
      this.gameActive = false;
      this.updateHighScore();
      this.score = 0;
      this.totalClickCount = 0;
      this.moles = [false, false, false, false];
    },
    gameLoop: function(lastTime) {
      const tick = (performance.now() - lastTime) / 1000;
      this.interval = this.interval - tick;
      console.log(this.interval);
      if (this.timer <= 0) {
        this.endGame();
        return;
      }
      this.timer = (
        this.gameTime -
        (performance.now() - this.startGameTime) / 1000
      ).toFixed(2);
      if (this.interval < 0) {
        this.interval = 3;
        this.activateRandomMole();
      }
      requestAnimationFrame(this.gameLoop.bind(this, performance.now()));
    },
    activateRandomMole: function() {
      const randomMoleIndex = Math.floor(Math.random() * this.moles.length);
      if (!this.moles[randomMoleIndex]) {
        this.activateMole(randomMoleIndex);
      }
    },
    toggleMole: function(moleId, shouldShow) {
      const moles = this.moles.slice();
      moles[moleId] = shouldShow;
      this.moles = moles;
    },
    activateMole: function(moleId) {
      this.toggleMole(moleId, true);
      setTimeout(() => this.deactivateMole(moleId), 1500);
    },
    deactivateMole: function(moleId) {
      this.toggleMole(moleId, false);
    },
    handleMoleWhack: function(moleId) {
      this.score = this.score + 1;
      this.deactivateMole(moleId);
      this.totalClickCount++;
    },
    handleMoleMiss: function() {
      this.totalClickCount++;
    },
    updateHighScore() {
      this.highScore = Math.max(this.highScore, this.score);
    }
  },
  computed: {
    hitPercentage: function() {
      return ((this.score / this.totalClickCount) * 100 || 0).toFixed(2);
    }
  }
};
</script>
<style>
.app-container > div {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10vh;
}
.counter-container {
  display: flex;
  flex-direction: row;
  margin: 10px;
}
</style>

