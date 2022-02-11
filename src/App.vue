<template>
  <Header />
  <div class="letter-grid">
    <WordRow v-for="(guess, i) in guesses" :key="i" :value="guess" :solution="solution" :submitted="i < currentGuessIndex" :guessedLetters="guessedLetters"/>
  </div>
  <div v-if="wonGame || lostGame" class="result-wrapper">
      <div class="result-modal" v-if="wonGame">
        <h2>🏆 You guessed it!</h2>
        <button @click="reloadPage">Guess new word</button>
      </div>
      <div class="result-modal"  v-else-if="lostGame">
        <h2>🤔 Out of guesses.</h2>
        <button @click="reloadPage">Guess new word</button>
      </div>
  </div>
  <SimpleKeyboard class="keyboard" @onKeyPress="handleInput" :guessedLetters="guessedLetters"/>
</template>

<script>
import Header from './components/Header.vue';
import SimpleKeyboard from './components/SimpleKeyboard.vue';
import WordRow from './components/WordRow.vue';
export default {
  name: 'App',
  components: {
    Header,
    SimpleKeyboard,
    WordRow,
  },
  data() {
    return {
      solution: "",
      guesses: ["","","","",""],
      currentGuessIndex: 0,
      guessedLetters:{
        miss: [],
        found: [],
        hint: []
      }
    }
  }, 
  mounted() {
    this.getWord();
    window.addEventListener("keyup", (e) => {
      e.preventDefault();
      let key =
        e.keyCode == 13
          ? "{enter}"
          : e.keyCode == 8 
          ? "{bksp}"
          :String.fromCharCode(e.keyCode).toLowerCase();
        this.handleInput(key);
    });
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    },
    wonGame(){
      return this.guesses[this.currentGuessIndex - 1] === this.solution;
    },
    lostGame(){
      return !this.wonGame && this.currentGuessIndex == 5;
    }
  },
  methods: {
    reloadPage(){
      window.location.reload()
    },
    handleInput(key) {
      if (this.currentGuessIndex == 5 || this.wonGame) {
        return
      }
      const currentGuess = this.guesses[this.currentGuessIndex];
      if (key == "{enter}") {
        // send guess
        if (currentGuess.length == 5) {
          this.currentGuessIndex ++;
          for (let i = 0; i < currentGuess.length; i++) {
            let c = currentGuess.charAt(i);
            if (c == this.solution.charAt(i)) {
              this.guessedLetters.found.push(c);
            } else if (this.solution.indexOf(c) != -1 ) {
              this.guessedLetters.hint.push(c);
            } else {
              this.guessedLetters.miss.push(c);
            }
          }
        }
      } else if (key == "{bksp}") {
        // remove last letter
        this.guesses[this.currentGuessIndex] = currentGuess.slice(0, -1);
      } else if (currentGuess.length < 5) {
        const alphaRegex = /[a-zA-Z]/;
        if (alphaRegex.test(key)) {
          this.guesses[this.currentGuessIndex] += key;
        }
      }
    },
    getWord: function () {
      var axios = require("axios").default;

      var self = this;

      var options = {
        method: 'GET',
        url: 'https://random-words5.p.rapidapi.com/getRandom',
        params: {wordLength: '5'},
        headers: {
          'x-rapidapi-host': 'random-words5.p.rapidapi.com',
          'x-rapidapi-key': '8151b45141msh87a2c2f575621f1p1aa175jsnc40e209a5d68'
        }
      };

      axios.request(options).then(function (response) {
        console.log(response.data);
        self.solution = response.data;
      }).catch(function (error) {
        console.error(error);
      });
    }
  }
}
</script>

<style>
body{
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-weight: 900;
  max-width: 560px;
  margin: auto;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.result-wrapper{
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 100;
  padding: 24px;
  box-sizing: border-box;
  background: rgba(255, 255, 255, 0.6);
}
.result-modal{
  background: white;
  width: 375px;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0px 7px 47px -9px #D0D0D8;

}
button{
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-left: 16px;
  padding-right: 16px;
  border-radius: 4px;
  border: none;
  background: #eee;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  font-size: 17px;
  font-weight: 600;
  margin: auto;
}
</style>
