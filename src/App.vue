<template>
  <div>
    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount"/>

    <template v-if="this.answers">
      <div>
        <h1 v-html="this.question">
        </h1>
      
        <template v-for="(answer, index) in this.answers" :key="index">
         
            <input 
              :disabled="this.ansxerSubmitted"
              type="radio"
              name="options"
              :value="answer"
              id=""
              v-model="this.chosen_answer">
            <label v-html="answer"></label><br>
         
        </template>
      
        <button v-if="!this.answerSubmitted" @click="submitAnswer" class="send" type="button" >Send</button>
      
        <section class="result" v-if="this.answerSubmitted">
          <template v-if="this.chosen_answer == this.correctAnswer">
            <h4 v-html="'&#9989; Congratulations, the answer ' + this.correctAnswer + 'is correct.'" ></h4>
          </template>
          <template v-else>
            <h4 v-html="'&#10060;  I\'m sorry, you picked the wrong answer. The correct is' + this.correctAnswer+ '.'"></h4>
          </template>
          <button @click="this.getNewQuestion()"  class="send" type="button">Next question</button>
        </section>
      </div>
  
  
    </template>
  </div>

  
</template>
<script>

let api = 'https://opentdb.com/api.php?amount=1&category=18'

import ScoreBoard from './components/ScoreBoard.vue';

export default {
  name: 'App',
  components: {
    ScoreBoard
  },
  data() {
      return {
        question: undefined,
        incorrectAnswer: undefined,
        correctAnswer: undefined,
        chosen_answer: undefined,
        answerSubmitted:false,
        winCount: 0,
        loseCount: 0
      }
  },
  methods: {
    submitAnswer() {
      if (!this.chosen_answer) {
        console.log("Pick One option")
      } else {
        this.answerSubmitted = true;
        if (this.chosen_answer === this.correctAnswer) {
          this.winCount++;
          console.log('You got it right');
        } else {
          this.loseCount++;
        }
      }
    },
    getNewQuestion() {
      this.chosen_answer= undefined,
      this.answerSubmitted=false,
      this.question=undefined
      this.axios
      .get(api).then((response) => {
      this.question = response.data.results[0].question
      this.incorrectAnswer = response.data.results[0].incorrect_answers
      this.correctAnswer = response.data.results[0].correct_answer
      console.log(response.data.results)
    })
    }
  },
  computed: {
    answers() {
      if (this.incorrectAnswer && this.correctAnswer) {
        // Copie profonde des mauvaises réponses
        var answers = JSON.parse(JSON.stringify(this.incorrectAnswer));
        // Insère la bonne réponse à une position aléatoire
        answers.splice((Math.round(Math.random() * answers.length)), 0, this.correctAnswer);
        return answers;
      } 
      // Si les réponses ne sont pas encore chargées, retourne un tableau vide
      return [];
    }
  },

  created() {
    this.axios.get(api).then((response) => {
      this.question = response.data.results[0].question
      this.incorrectAnswer = response.data.results[0].incorrect_answers
      this.correctAnswer = response.data.results[0].correct_answer
      console.log(response.data.results)
    })
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
  margin: 60px auto;
  max-width: 960px;

  input[type=radio] {
    margin: 12px 4px;
  }

  button.send {
    height: 40px;
    min-width: 120px;
    background-color: crimson;
    color: #fff;
    border: 1pw solid #1867c0;
  }
}
</style>
