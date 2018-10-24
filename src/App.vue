<template>
  <div class="whackamole">
    <h1 class="logo">
      Whack-a-mole!
    </h1>
    <button class="start-game" @click='startGame'>
      Start Game
    </button>
    <div class="counters-container">
      <Counter label='Score' v-bind:count='score'></Counter>
      <Counter label='HighScore' v-bind:count='highScore'></Counter>
      <Counter label='Time' v-bind:count='time'></Counter>
    </div>
    <div class="moles-container"　v-bind:class="getClassObj">
      <Mole v-for='(item, index) in moles' v-bind:active='item' v-bind:key='index'></Mole>
    </div>
  </div>
</template>

<script>
import Counter from './components/Counter';
import Mole from './components/Mole';

export default {
  name: 'App',
  components: {
    Counter,
    Mole,
  },
  data: () => {
    return {
      score: 0,
      highScore: 0,
      time: 20,
      moles: [false, false, false, false],
      gameActive: false,
    }
  },
  computed: {
    getClassObj: function() {
      return {
        'game-active': this.gameActive,
      };
    }
  },
  methods: {
    resetState: function() {
      this.score = 0;
      this.time = 20;
      this.moles = [false, false, false, false];
    },
    startGame: function() {
      this.resetState();
      this.gameActive = true;
      this.startTimer();
    },
    endGame: function() {
      this.gameActive = false;
      this.stopTimer();
    },
    startTimer: function() {
      this.timerId = setInterval(() => {
        this.countTime();
      }, 1000);
    },
    countTime: function() {
      this.time--;
      if (this.time === 0) {
        this.endGame();
      }
    },
    stopTimer: function() {
      clearInterval(this.timerId);
    },
  },
};
</script>

<style>
.whackamole {
  font-family: 'Bungee', sans-serif;
  max-width: 960px;
  margin: auto;
  text-align: center;
}

.start-game {
  font-family: 'Bungee', sans-serif;
  padding: 20px;
  border-radius: 3px;
  border: 0;
  background-color: #52b1d6;
  color: #fff;
  font-size: 1em;
  cursor: pointer;
}

.counters-container {
  display: flex;
  justify-content: space-evenly;
}

.moles-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  opacity: 0.5;
  transition: opacity 0.3s ease;
}

.moles-container.game-active {
  opacity: 1;
}

</style>
