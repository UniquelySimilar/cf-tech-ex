<template>
  <div class="quiz container">
    <h2>QUESTIONS</h2>
    <p>Please answer the following {{ numQuestions }} questions, presented one at a time.
      Questions can not be skipped, and you cannot return to a previous question.
    </p>
    <hr>
    <p>{{ currentQuestion.text }}</p>
    <div v-for="(answer,index) in currentQuestion.answers" :key="index">
      <input type="radio" :value="answer" v-model="selectedAnswer">
      <label>{{ answer }}</label>
    </div>
    <span>Answer: {{ questionsExt[currentIdx].answer }}</span>
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
      selectedAnswer: null
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
  watch: {
    selectedAnswer() {
      this.questionsExt[this.currentIdx].answer = this.selectedAnswer;
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
    selectAnswer(event) {
      console.log(event);
    }
  }
}
</script>

<style scoped>
  input[type="radio"] {
    margin-right: 1em;
  }
</style>