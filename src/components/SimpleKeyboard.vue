<template>
  <div class="simple-keyboard"></div>
</template>

<script>
import Keyboard from "simple-keyboard";
import "simple-keyboard/build/css/index.css"

export default {
  name: 'SimpleKeyboard',
  props:{
      guessedLetters: Object,
  },
  data() {
    return {
        keyboard: null
    }
  },
  mounted() {
    this.keyboard = new Keyboard("simple-keyboard", {
      onKeyPress: this.onKeyPress,
      layout: {
        default: [
            "q w e r t y u i o p",
            "a s d f g h j k l",
            "{enter} z x c v b n m {bksp}",
            ],
        },
        display: {
        '{bksp}': 'delete',
        '{enter}': 'enter',
        }
    });
  },
  methods: {
    // onChange(input) {
    //   this.$emit("onChange", input);
    // },
    onKeyPress(button) {
      this.$emit("onKeyPress", button);

      /**
       * If you want to handle the shift and caps lock buttons
       */
      if (button === "{shift}" || button === "{lock}") this.handleShift();
    },
    handleShift() {
      let currentLayout = this.keyboard.options.layoutName;
      let shiftToggle = currentLayout === "default" ? "shift" : "default";

      this.keyboard.setOptions({
        layoutName: shiftToggle
      });
    }
  },
  watch: {
    input(input) {
      this.keyboard.setInput(input);
    },
    guessedLetters: {
        handler(guessedLetters){
            
            let missedLetters = guessedLetters.miss.join(' ');
            let foundLetters = guessedLetters.found.join(' ');
            let hintLetters = guessedLetters.hint.join(' ');
            
            this.keyboard.addButtonTheme(missedLetters, 'miss');
            this.keyboard.addButtonTheme(foundLetters,'found');
            this.keyboard.addButtonTheme(hintLetters,'hint');
        },
        deep: true
    },
    // guessedLetters(guessedLetters) { // watch it
    //     console.log(guessedLetters);
    //     this.keyboard.value.addButtonTheme(
    //         guessedLetters.miss.join(''),
    //         'miss'
    //     );
    //     this.keyboard.value.addButtonTheme(
    //         guessedLetters.founD.join(''),
    //         'found'
    //     );
    //     this.keyboard.value.addButtonTheme(
    //         guessedLetters.hint.join(''),
    //         'hint'
    //     );
    // }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.simple-keyboard{
  width: 100%;
  position: relative;
  bottom: 0;
}
.hg-theme-default .hg-button {
    font-size: 17px;
    height: 64px;
    text-transform: uppercase;
}
.miss{
    background: rgb(120, 124, 126) !important;
    color: white;
}
.found{
    background: rgb(106, 170, 100) !important;
    color: white;
}
.hint:not(.found){
    background: rgb(201, 180, 88) !important;
    color: white;
}
</style>