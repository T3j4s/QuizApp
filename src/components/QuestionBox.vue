<template>
  <div>
    <b-container>
      <b-row>
        <b-col sm="6" class="offset-3">
          <b-jumbotron>
            <p>{{ currentQuestions.question }}</p>

            <hr class="my-4" />

            <b-list-group v-for="(answer, index) in answers" :key="index">
              <b-list-group-item
                :class="[
                  !isAnswered && selectedAnswer == index
                    ? 'selected'
                    : isAnswered && correctAnswerIndex == index
                    ? 'correct'
                    : isAnswered &&
                      selectedAnswer == index &&
                      correctAnswerIndex !== index
                    ? 'incorrect'
                    : ''
                ]"
                @click.prevent="selectAnswer(index)"
              >
                {{ answer }}
              </b-list-group-item>
            </b-list-group>

            <b-button
              @click="submit"
              :disabled="selectedAnswer == null || isAnswered"
              variant="primary"
              >Submit</b-button
            >
            <b-button
              variant="success"
              v-if="
                counter == 9 ? (buttonText = 'Finiish') : (buttonText = 'Next')
              "
              @click="next"
              >{{ buttonText }}</b-button
            >
          </b-jumbotron>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestions: Object,
    next: Function,
    increment: Function,
    counter: Number
  },
  data() {
    return {
      buttonText: "Next",
      selectedAnswer: null,
      shuffledAnswers: [],
      correctAnswerIndex: null,
      isAnswered: false
    };
  },
  methods: {
    selectAnswer(index) {
      this.selectedAnswer = index;
    },
    shuffleAnswer() {
      let answer = [
        ...this.currentQuestions.incorrect_answers,
        this.currentQuestions.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answer);
      this.correctAnswerIndex = this.shuffledAnswers.indexOf(
        this.currentQuestions.correct_answer
      );
    },
    submit() {
      let isCorrect = false;
      if (this.selectedAnswer == this.correctAnswerIndex) {
        isCorrect = true;
      }
      this.isAnswered = true;
      this.increment(isCorrect);
    }
  },
  computed: {
    answers() {
      let answer = [...this.currentQuestions.incorrect_answers];
      answer.push(this.currentQuestions.correct_answer);
      return answer;
    }
  },
  watch: {
    currentQuestions: {
      immediate: true,
      handler() {
        this.selectedAnswer = null;
        this.shuffleAnswer();
        this.isAnswered = false;
      }
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 10px;
}
.list-group-item:hover {
  background-color: #b0bec5;
  cursor: pointer;
}
.btn {
  margin: 0px 5px;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: #ef5350;
}
</style>
