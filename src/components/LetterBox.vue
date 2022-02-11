<template>
    <div class="letterbox" :class="{'focus': result == '' && letter.length > 0,
      'pulse': pulse, 
      'found': result == 'found', 
      'hint': result == 'hint', 
      'miss': result == 'miss'}"
      >{{letter}}</div>
</template>

<script>

export default {
  name: 'LetterBox',
  props:{
      letter: {
          type: String,
          default: "",
      },
      result:{
          type: String,
          default: "",
      }
  },
  data() {
    return {
        pulse: false,
    }
  },
  mounted() {

  },
  methods: {
   
  },
  watch: {
      letter(selected){
        if (selected && selected.length > 0) {
            this.pulse = true;
            setTimeout(() => {
                this.pulse = false;
            }, 100);
        }
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.letterbox{
    border: 2px solid #eee;
    margin: 2px;
    height: 64px;
    width: 64px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    text-align: center;
    text-transform: uppercase;
    transition: all 0.6s ease;
    border-radius: 8px;
}
.pulse{
    transform: scale(1.7);
}
.focus{
    border-color: #8E8E8E;
}
</style>