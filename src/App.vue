<template>
  <div id="app">
    <Header :correctCount="correctCount" :totalCount="totalCount" />
    <QuestionBox
      v-if="questions.length"
      :hidden="completed"
      :currentQuestions="questions[index]"
      :next="next"
      :increment="increment"
      :counter="counter"
    />
    <Result
      :hidden="!completed"
      :correctCount="correctCount"
      :totalCount="totalCount"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import Result from "./components/Result.vue";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox,
    Result
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctCount: 0,
      totalCount: 1,
      counter: 0,
      completed: false
    };
  },
  methods: {
    next() {
      this.index++;
      this.totalCount++;
      this.counter++;
      if (this.counter == 10) {
        this.totalCount--;
        this.completed = true;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.correctCount++;
      }
    }
  },
  mounted: function() {
    fetch(
      "https://opentdb.com/api.php?amount=10&category=18&difficulty=medium&type=multiple",
      { method: "get" }
    )
      .then(res => {
        return res.json();
      })
      .then(data => {
        this.questions = data.results;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
