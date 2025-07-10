<template>
  <div>
    <ScoreBoard></ScoreBoard>
    <template v-if="this.answers">
      <h1 v-html="this.question"></h1>
      <template v-for="(answer, index) in this.answers" :key="index">
        <input type="radio" name="options" :value="answer" v-model="this.chosenAnswer" />
        <label v-html="answer"></label><br />
      </template>
      <button v-if="!this.answerSubmitted" class="send" type="button" @click="checkAnswer()"
        :disabled="this.answerSubmitted">Enviar</button>
      <section class="result">
        <h4 v-if="this.chosenAnswer === this.correctAnswer && this.answerSubmitted">
          ✔️ Parabéns, você acertou! +1 ponto para o jogador
        </h4>
        <h4 v-else-if="this.answerSubmitted">
          ❌ Que pena, você errou! +1 ponto para o computador
        </h4>
        <button @click="getNewQuestion()" class="send" type="button">Nova pergunta</button>
      </section>
    </template>
  </div>
</template>
<script>
import ScoreBoard from "./components/ScoreBoard.vue";

const url = 'https://opentdb.com/api.php?amount=1';

function embaralharArray(array) {
  for (let index = array.length - 1; index > 0; index--) {
    const randomNumb = Math.floor(Math.random() * (index + 1)); // o j é randomNumb
    [array[index], array[randomNumb]] = [array[randomNumb], array[index]];
  }
  return array;
}
export default {
  name: 'App',
  components: {
    ScoreBoard,
  },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
    }
  },
  computed: {
    answers() {
      if (!this.incorrectAnswers || !this.correctAnswer) {
        return undefined;
      }
      let answers = [
        ...this.incorrectAnswers,
        this.correctAnswer
      ]

      return embaralharArray(answers);
    },
  },
  methods: {
    checkAnswer() {
      if (!this.chosenAnswer) {
        alert('Escolha uma resposta antes de enviar!'); // funciona esse tipo de alerta, mas não é o ideal
        return;
      }
      this.answerSubmitted = true;
      if (this.chosenAnswer === this.correctAnswer) {
        console.log('Certo! +1 ponto para o jogador');

      } else {
        console.log('Errado! +1 ponto para o computador');
      }
    },
    getNewQuestion() {
      if (this.answerSubmitted) {
        this.chosenAnswer = undefined;
        this.answerSubmitted = false;
      }
      fetch(url)
        .then(response => response.json())
        .then(data => {
          this.question = data.results[0].question;
          this.incorrectAnswers = data.results[0].incorrect_answers;
          this.correctAnswer = data.results[0].correct_answer;
        })
        .catch(error => {
          console.error('Error fetching trivia questions:', error);
        });

    }
  },
  created() {
    this.getNewQuestion()
  },

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
}

input[type='radio'] {
  margin: 12px 4px;
}

button.send {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;
}
</style>
