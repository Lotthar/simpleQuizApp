<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">{{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group v-for="(answer, index) in answers" :key="index">
        <b-list-group-item :class="answerClass(index)" @click="selectAnswer(index)">{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button
        :disabled="selectedIndex === null || answered"
        variant="primary"
        @click="submitAnswer"
        href="#"
      >Submit</b-button>
      <b-button :disabled="isSubmited" variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _lo from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      answered: false,
      correctIndex: null,
      isSubmited: true
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.isSubmited = true;
        this.shuffleAnswers();
      }
    }
    // () {
    //   this.selectedIndex = null;
    //   this.shuffleAnswers();
    // }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _lo.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.isSubmited = false;
      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.answered && this.selectedIndex === index)
        answerClass = "selected";
      else if (this.answered && this.correctIndex === index)
        answerClass = "correct";
      else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      )
        answerClass = "incorrect";
      return answerClass;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  cursor: pointer;
  background: rgba(0, 0, 0, 0.1);
}
.btn {
  margin: 0 15px;
}

.selected {
  background: rgba(0, 0, 0, 0.1);
}

.correct {
  background: green;
}

.incorrect {
  background: red;
}
</style>
