<template>
  <div class="game-container">
    <div class="timer">倒计时：{{ timeLeft }} 秒</div>
    <div class="letter-container">
      <span
        v-for="(letter, index) in letters"
        :key="index"
        :class="{ correct: index < correctIndex, current: index === correctIndex }"
      >
        {{ letter }}
      </span>
    </div>
    <div class="stats">
      <p>准确率：{{ accuracy }}%</p>
      <p>错误个数：{{ errors }}</p>
      <p>输入总数：{{ totalTyped }}</p>
    </div>
    <div v-if="gameOver" class="game-over">游戏结束</div>
    <button v-if="gameOver" class="restart-button" @click="restartGame">再来一局</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeLeft: 10,
      letters: [],
      correctIndex: 0,
      errors: 0,
      totalTyped: 0,
      correctTyped: 0,
      gameOver: false,
    };
  },
  computed: {
    accuracy() {
      return this.totalTyped > 0 ? Math.round((this.correctTyped / this.totalTyped) * 100) : 0;
    },
  },
  methods: {
    startGame() {
      this.generateLetters();
      this.timer = setInterval(() => {
        if (this.timeLeft > 0) {
          this.timeLeft -= 1;
        } else {
          clearInterval(this.timer);
          this.gameOver = true;
        }
      }, 1000);
    },
    generateLetters() {
      const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
      this.letters = Array.from({ length: 10 }, () => letters[Math.floor(Math.random() * letters.length)]);
      this.correctIndex = 0;
    },
    handleKeyInput(event) {
      if (!this.gameOver && this.correctIndex < this.letters.length) {
        const inputLetter = event.key.toUpperCase();
        const correctLetter = this.letters[this.correctIndex];

        this.totalTyped += 1;
        if (inputLetter === correctLetter) {
          this.correctTyped += 1;
          this.correctIndex += 1;

          // 如果所有字母都输入正确，重新生成一排字母
          if (this.correctIndex === this.letters.length) {
            this.generateLetters();
          }
        } else {
          this.errors += 1;
        }
      }
    },
    restartGame() {
      this.gameOver = false;
      this.timeLeft = 10;
      this.errors = 0;
      this.totalTyped = 0;
      this.correctTyped = 0;
      this.generateLetters();
      clearInterval(this.timer);
      this.startGame();
    },
  },
  mounted() {
    this.startGame();
    window.addEventListener('keydown', this.handleKeyInput);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.handleKeyInput);
    clearInterval(this.timer);
  },
};
</script>

<style scoped>
.game-container {
  text-align: center;
  font-family: Arial, sans-serif;
}
.timer {
  font-size: 24px;
  margin-top: 20px;
}
.letter-container {
  font-size: 48px;
  margin-top: 40px;
}
.correct {
  color: green;
}
.current {
  color: blue;
}
.stats {
  margin-top: 30px;
}
.game-over {
  font-size: 36px;
  color: red;
  margin-top: 20px;
}
.restart-button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 18px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.restart-button:hover {
  background-color: #45a049;
}
</style>