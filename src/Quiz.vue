<template>
  <div class="quiz container">
    <div class="questions" v-if="!displaySummary">
      <h3>QUESTIONS</h3>
      <p>Please answer the following {{ numQuestions }} questions, presented one at a time.
        Questions cannot be skipped, and you cannot return to a previous question.
      </p>
      <hr>
      <p class="font-weight-bold">{{ currentQuestion.text }}</p>
      <div v-for="(answer,index) in currentQuestion.answers" :key="index">
        <input type="radio" :value="answer" v-model="selectedAnswer">
        <label>{{ answer }}</label>
      </div>
      <div>
        <button @click="submitAnswer">Submit</button>
        <span class="warning" v-if="showWarning">You must select an answer</span>
      </div>
    </div>

    <div class="summary" v-if="displaySummary">
      <h3>SUMMARY</h3>
      <hr>
      <div v-for="question in questionsExt" :key="question.id">
        <p>
          <span class="font-weight-bold">{{ question.text }}</span>
          <br>
          <span>{{ question.answer }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      questionsExt: this.addQuestionProperties(),
      currentIdx: 0,
      selectedAnswer: null,
      showWarning: false,
      displaySummary: false
    };
  },
  computed: {
    numQuestions() {
      return this.questions.length;
    },
    currentQuestion() {
      return this.questionsExt[this.currentIdx];
    }
  },
  methods: {
    addQuestionProperties() {
      let questionsExt = this.questions.map( (question) => {
        let questionExt = { ...question, answer: null };
        return questionExt;
      });

      return questionsExt;
    },
    submitAnswer() {
      // User must answer current question
      if (this.selectedAnswer === null) {
        this.showWarning = true;
        return;
      }

      // Update question object with answer
      this.currentQuestion.answer = this.selectedAnswer;

      // Reset
      this.selectedAnswer = null;
      this.showWarning = false;

      // Select next question if available, or show summary
      if (this.currentIdx < this.questionsExt.length - 1) {
        this.currentIdx++;
      }
      else {
        this.displaySummary = true;
      }
    }
  }
}
</script>

<style scoped>
  .questions, .summary {
    margin-top: 1em;
  }

  input[type="radio"], button {
    margin-right: 1em;
  }

  .warning {
    color: red;
  }
</style>