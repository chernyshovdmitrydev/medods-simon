<script>
import SimonButton from "./components/SimonButton.vue";
export default {
  components: {
    SimonButton,
  },
  data() {
    return {
      queue: [],
      round: 1,
      colors: ["green", "red", "yellow", "blue"],
      currentIndex: 0,
      isBlocked: true,
      difficulty: "easy",
    };
  },
  methods: {
    startGame() {
      this.queue = [];
      this.round = 1;
      this.playRound();
    },
    pushRandomColor() {
      const randomNumber = Math.floor(Math.random() * 4);
      const randomColor = this.colors[randomNumber];
      this.queue.push(randomColor);
    },
    getDifficultyTime() {
      switch (this.difficulty) {
        case "easy":
          return 1500;
        case "medium":
          return 1000;
        case "hard":
          return 400;
      }
    },
    playRound() {
      this.isBlocked = true;
      this.pushRandomColor();
      let index = 0;
      const interval = setInterval(() => {
        if (index < this.queue.length) {
          const color = this.queue[index];
          this.$refs[`${color}Button`].beep();
          this.$refs[`${color}Button`].blink();
          index++;
        } else {
          this.isBlocked = false;
          clearInterval(interval);
        }
      }, this.getDifficultyTime());
    },
    simonButtonClick(color) {
      if (color === this.queue[this.currentIndex]) {
        this.$refs[`${color}Button`].beep();
        this.$refs[`${color}Button`].blink();
        this.currentIndex++;
        if (this.currentIndex === this.queue.length) {
          this.currentIndex = 0;
          this.round++;
          this.playRound();
        }
      } else {
        this.endGame();
      }
    },
    endGame() {
      this.currentIndex = 0;
      this.isBlocked = true;
      this.queue = [];
      const errorAudio = new Audio(`/medods-simon/audio/error.mp3`);
      errorAudio.volume = 0.2;
      errorAudio.play();
      for (let i = 0; i < 3; i++) {
        setTimeout(() => {
          for (let color of this.colors) {
            this.$refs[`${color}Button`].blink();
          }
        }, 600 * i);
      }
    },
    toggleDifficulty() {
      switch (this.difficulty) {
        case "easy":
          this.difficulty = "medium";
          break;
        case "medium":
          this.difficulty = "hard";
          break;
        case "hard":
          this.difficulty = "easy";
          break;
      }
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="game">
      <SimonButton
        color="green"
        ref="greenButton"
        @simonButton-click="simonButtonClick"
        :isBlocked="isBlocked"
      />
      <SimonButton
        color="red"
        ref="redButton"
        @simonButton-click="simonButtonClick"
        :isBlocked="isBlocked"
      />
      <SimonButton
        color="yellow"
        ref="yellowButton"
        @simonButton-click="simonButtonClick"
        :isBlocked="isBlocked"
      />
      <SimonButton
        color="blue"
        ref="blueButton"
        @simonButton-click="simonButtonClick"
        :isBlocked="isBlocked"
      />
      <div class="round">{{ round }}</div>
      <button class="intarface-button start-button" @click="startGame">
        START
      </button>
      <button
        :disabled="queue.length"
        class="intarface-button difficulty-button"
        @click="toggleDifficulty"
      >
        {{ difficulty }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 30em;
  height: 30em;
  height: calc(100vh - 64px);
  background-color: #505050;
  border-radius: 20px;
  box-shadow: 0 0 10px 3px rgba(0, 0, 0, 0.1);
  padding: 2%;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.game {
  display: grid;
  background-color: #101010;
  border-radius: 50%;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
}

.game:before {
  content: "";
  position: absolute;
  background-color: #101010;
  height: 13em;
  width: 13em;
  z-index: 1;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  border-radius: 50%;
}

.round {
  background-color: white;
  font-size: 12px;
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  height: 2em;
  width: 3em;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 30%;
  z-index: 2;
}

.intarface-button {
  background-color: red;
  font-size: 10px;
  position: absolute;
  transform: translate(-50%, -50%);
  z-index: 3;
  cursor: pointer;
}
.start-button {
  top: 60%;
  left: 50%;
}

.difficulty-button {
  top: 65%;
  left: 50%;
}
</style>
