<template>
  <div class="game-board">
    <button id="blue" class="blue" v-bind:class="{ active: isActive.blue}"
    @click="clickColor"></button>
    <button id="red" class="red" v-bind:class="{ active: isActive.red}"
    @click="clickColor"></button>
    <button id="green" class="green" v-bind:class="{ active: isActive.green}"
    @click="clickColor"></button>
    <button id="yellow" class="yellow" v-bind:class="{ active: isActive.yellow}"
    @click="clickColor"></button>
  </div>
</template>

<script>
export default {
  name: 'GameBoard',
  data:() =>({
    copyColors:[],
    sounds: {
      blue: null,
      green: null,
      red: null,
      yellow: null,
    },
    isActive: {
      "blue": false,
      "red": false,
      "green": false,
      "yellow": false,
      },
    isCanPushButton: false,
    interval: null,
  }),
  props: {
    colors: {
      type: Array,
      },
    timerDifficult: {
      type: Number,
    },
    typeGame:{
      type: String,
    },
  },
  watch: {
    colors: function() {
        let i = 0;
        this.copyColors = [];
        this.interval = setInterval(() => {
          let curColor = this.colors[i];
          this.copyColors.push(curColor);
          this.lightUp(curColor);
          this.soundUp(curColor);
          i++;
          if (i >= this.colors.length){
            this.isCanPushButton = true;
            clearInterval(this.interval);
          }

        }, this.timerDifficult);
    }
  },
  methods:{
    lightUp(color){
      if(this.typeGame === "sound") return;
      this.isActive[color] = true;
      setTimeout(() => {
        this.isActive[color] = false;
      }, 250);
    },
    soundUp(color){
      if(this.typeGame === "light") return;
      if(color){
      let audio = new Audio();
      audio.src = require(`../assets/sounds/${color}.mp3`);
      audio.play();}
    },
    clickColor(event){
      if(!this.isCanPushButton) return;
      let clickColor = event.target.id;
      this.soundUp(clickColor);
      this.checkColor(clickColor);
      this.checkEndRound();
    },
    checkColor(color){
      if(this.copyColors[0] == color)
        this.copyColors.shift(); 
      else {
        clearInterval(this.interval);
        this.isCanPushButton = false;
        this.$emit("endGame");
      }
    },
    checkEndRound(){
      if(!this.copyColors.length)
      { 
        clearInterval(this.interval);
        this.isCanPushButton = false;
        this.$emit("nextLevel");
        }
    }
  },
}
</script>

<style scoped>
.game-board{
  position: relative;
  width: 360px;
  height: 360px;
  border-radius: 50%;
  border: 3px solid #fff;
  box-shadow: 0 0 10px #000;
}

button{
  margin: 0;
  padding: 0;
  border: 0;
  width: 180px;
  height: 180px;
  opacity: 0.6;
}

.red{
  position: absolute;
  right: 0;
  border-top-right-radius: 100%;
  background: #FF5643;
}

.blue{
  border-top-left-radius: 100%;
  position: absolute;
  left: 0;
  background: dodgerblue;
}

.green{
  position: absolute;
  bottom: 0;
  right: 0;
  border-bottom-right-radius: 100%;
  background: #BEDE15;
}

.yellow{
  position: absolute;
  bottom: 0;
  left: 0;
  border-bottom-left-radius: 100%;
  background: #FEEF33;
}

.red:hover, .blue:hover, .yellow:hover, .green:hover {
	border: 2px solid rgb(30, 30, 30);
} 

.red:active, .blue:active, .yellow:active, .green:active {
	border: 1px solid #fff;
  opacity: 1;
}

.active{
  opacity: 1;
}

</style>
