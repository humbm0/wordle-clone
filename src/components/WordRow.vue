<template>
<div class="wordrow"><LetterBox v-for="i in 5" :key="i" :letter="value[i-1]" :result="results[i-1]" /> </div>
</template>

<script>
import LetterBox from '../components/LetterBox.vue';
export default {
  name: 'WordRow',
  components: {
    LetterBox
  },
  props:{
    value: String,
    solution: String,
    submitted: Boolean,
    guessedLetters: Object
  },
  data() {
    return {
        results: ["", "", "", "", "", ""]
    }
  },
  mounted() {

  },
  methods: {
   
  },
  watch: {
      async submitted(){

          if (this.submitted) {
            let s = this.solution;
            let v = this.value;

            let temp = ["miss", "miss", "miss", "miss", "miss"  ];
            let letterPool = [];

            for (let i = 0; i < 5; i++) {
                if (s.charAt(i) == v.charAt(i)) {
                    temp[i] = "found"
                } else {
                    letterPool.push(s.charAt(i));
                }
            }
            for (let i = 0; i < 5; i++) {
                if (temp[i] == "miss"){
                    if (letterPool.indexOf(v.charAt(i)) != -1) {
                        letterPool.splice(letterPool.indexOf(v.charAt(i)), 1)
                        temp[i] = "hint";
                    }
                }
                this.results[i] = temp[i];
                await new Promise((resolve) => setTimeout(resolve, 500));
            }
          }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .wordrow{
        display: flex;
        margin: auto;
        justify-content: center;
    }
</style>