<template>
  <div>
     <h1 v-html="question"></h1>

     <template v-if="this.question">
       <template v-for="answer in answers" :key="answer">
         <input  
            type="radio" 
            name="options" 
            :value="answer"
            :disabled="answerSubmitted"
            v-model="this.chosenAnswer"
            >
         <label v-html="answer"></label><br>
       </template>
     
       <button @click="this.submitAnswer()" v-if="!this.answerSubmitted" class="send" type="button">Select Answer</button>
     
       <section class="result" v-if="answerSubmitted">
         <button @click="newAnswer" class="send" type="button">Next Question</button>
         <h4 v-if="this.chosenAnswer === this.correctAnswer">Congratulations, the answer "<span v-html="this.correctAnswer"></span>" is correct.</h4>
         <h4 v-else>Your answer was incorrect. the right answer was "<span v-html="this.correctAnswer"></span>".</h4>
       </section>

      </template>

  </div>
</template>

<script>

export default {
  name: 'App',
  
  data() {
    return {
      question: undefined,
      incorrectAnswers: [],
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.incorrectAnswers];
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert('Please choose an answer');
        return;
      }
      this.answerSubmitted = true;
      if (this.chosenAnswer === this.correctAnswer) {
        console.log('Correct answer');
      } else {
        console.log('Incorrect answer');
      }
    },
    newAnswer() {
      this.answerSubmitted = false;

      this.axios.get('https://opentdb.com/api.php?amount=1&category=18')
      .then(response => {
        this.question = response.data.results[0].question;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
      })
      .catch(error => {
        console.log(error);
      });
    }
  },
  created() {
    this.newAnswer();
  }

}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px auto;
  max-width: 960px;

  input[type="radio"] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
