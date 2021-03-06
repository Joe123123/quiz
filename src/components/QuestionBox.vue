<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question | parse }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
          :disabled="answered"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false,
    };
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let a = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      for (let i = a.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
      }
      this.correctIndex = a.indexOf(this.currentQuestion.correct_answer);
      this.shuffledAnswers = a.map((answer) =>
        answer.replace(/&quot;/g, '"').replace(/(&#039;|&rsquo;)/g, "'")
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerClass(index) {
      return {
        selected: this.selectedIndex === index && !this.answered,
        correct: this.correctIndex === index && this.answered,
        incorrect:
          this.selectedIndex === index &&
          this.correctIndex !== index &&
          this.answered,
      };
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
  },
  filters: {
    parse: function(value) {
      return value.replace(/&quot;/g, '"').replace(/(&#039;|&rsquo;)/g, "'");
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background: lightblue;
}

.correct {
  background: lightgreen;
}

.incorrect {
  background: red;
}
</style>
