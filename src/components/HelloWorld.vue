<template>
  <div class="container">
    <h4>{{word}}</h4>
    <!-- <div class="grid">
      <div class="row" v-for="attempt in attempts" :key="attempt">
        <input v-for="letter in attempt.attempt" :key="letter" type="text" maxlength="1" class="letter-block" v-model="letter.letter" @input="setValue" v-on:keyup.enter="validateLetters">
      </div>
    </div> -->
  <SimpleKeyboard/>
  </div>
</template>

<script>
import SimpleKeyboard from '../components/SimpleKeyboard.vue';

export default {
  components: { SimpleKeyboard },
  name: 'HelloWorld',
  data() {
    return {
      solution: "tests",
      guesses: ["","","","",""],
    }
  },
  mounted() {
    // this.getWord();
    // this.keyPress();
    // this.saveAttempt();
  },
  methods: {
    // saveAttempt: function(){
    //   window.addEventListener('keydown', (e) => {
    //     var charCode = e.keyCode;
    //     if (this.attempt.length == this.wordLength && charCode == 13) {
    //       this.attempts.push(this.attempt);
    //       console.log(this.attempts);  
    //     }
    //   });
    // },
    // nextField: function (event){
    //   // if (this.attempt.length < 5) {
    //   //  event.target.nextElementSibling.focus(); 
    //   // }
    //   event.target.nextElementSibling.focus(); 
    // },
    // setValue: function(){
    //   if (this.letterIndex < 5) {
    //     console.log(this.attempts[this.attemptIndex].attempt[this.letterIndex].letter);
    //     this.letterIndex ++;
    //     if (this.letterIndex <= 4) {
    //       event.target.nextElementSibling.focus(); 
    //     }
    //   }
    // },
    // validateLetters: function(){
    //   console.log(this.attempts);
    //   event.target.parentElement.nextElementSibling.firstElementChild.focus();
      
    //   this.letterIndex = 0;
    //   this.attemptIndex ++;

    //   var word = this.word;
    //   var attempt = this.attempt;

    //   for (var i = 0; i < word.length; i++) {
    //       for (var j = 0; j < attempt.length; j++) {
    //           if (word[i] == attempt[j]) {
    //             console.log(word[i] + "&" + attempt[j]);
    //           }
    //       }
    //   }

    //   if (letter == this.word[this.index]) {
    //     console.log("correct letter");
    //   } else{
    //     console.log("incorrect letter");
    //   }
    // },
    // keyPress: function (){
    //   window.addEventListener('keydown', (e) => {
    //     var charCode = e.keyCode;

    //     if (charCode == 8) {
    //         this.attempt.pop();
    //     } else if (this.attempt.length < this.wordLength) {
    //       if ((charCode > 64 && charCode < 91) || (charCode > 96 && charCode < 123)){
    //         this.attempt.push(e.key);
    //         console.log(this.attempt);
    //       }else{
    //         return false
    //       } 
    //     }
        
    //   });
    // },
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
        self.word = response.data;
      }).catch(function (error) {
        console.error(error);
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-content: center;
  margin: auto;
}
.grid{
  display: flex;
  flex-direction: column;
  align-self: center;
}
.row{
  display: flex;
  flex-direction: row;
}
.letters{
  display: flex;
  align-self: center;
}
.letter-block{
  border: 2px solid #eee;
  margin: 2px;
  height: 40px;
  width: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  text-align: center;
}
.letter{

}
</style>
