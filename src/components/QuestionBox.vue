<template>
<div class="question-box-container">
  <b-jumbotron>
    <template v-slot:lead>
     {{ currentQuestion.question }}
    </template>

    <hr class="my-4">
    <b-list-group>
      <b-list-group-item 
        v-for="(answer, index) in shuffledAnswers" :key="index"
        @click.prevent="selectAnswer(index)"
        :class="[selectedIndex === index ? 'selected' : '']"        
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
    <b-button @click="next" variant="success">
      Next
    </b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next : Function,
        increment: Function
    },
    data() {
      return {
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false
      }
    },
    computed: {
      answers() {
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return answers
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler() {
          this.selectedIndex = null
          this.answered = false
          this.suffleAnswers()
        }
      }
    },
    methods: {
      selectAnswer(index) {
        this.selectedIndex = index
      },
      submitAnswer() {
        let isCorrect = false

        if(this.selectedIndex === this.correctIndex) {
          isCorrect = true
        }
        this.answered = true
        this.increment(isCorrect)
      },
      suffleAnswers() {
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      }
    }
}
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
    background-color: lightblue;
  }
  .correct {
    background-color: lightgreen;
  }
  .incorrect {
    background-color: red;
  }
  

</style>>
