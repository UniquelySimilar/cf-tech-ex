<template>
  <div class="quiz container">
    <h2>QUESTIONS</h2>
    <p>Please answer the following {{ numQuestions }} questions, presented one at a time.
      Questions can not be skipped, and you cannot return to a previous question.
    </p>
    <ul>
      <li v-for="question in questionsExt" :key="question.id">
        {{ question.id }} - {{ question.text }}
      </li>
    </ul>
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
      questionsExt: this.addQuestionProperties()
    };
  },
  computed: {
    numQuestions() {
      return this.questions.length;
    },
    // questionsExt() {
    //   return this.addQuestionProperties()
    // }
  },
  methods: {
    addQuestionProperties() {
      let questionsExt = this.questions.map( (question, index) => {
        let questionExt = { ...question, id: index + 1, answer: null };
        return questionExt;
      });

      return questionsExt;
    }    
  }
}
</script>