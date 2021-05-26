<template>
  <div class="wrapper" >
    <h1>Simon Game</h1>
    <div class="game-over" v-if=isGameOver> GAME OVER!</div>
    <div class="level">Level: {{level}}</div>
    <div class="game-flex">
      <GameBoard 
      :colors=curColorSequence 
      :typeGame=curTypeGame
      :timerDifficult=curTimerDifficult 
      :isRun=isRunning
      @endGame=onGameOver 
      @nextLevel=onNextLevel />
      <GameOptions 
      @changeCurDifficult=onChangeCurDifficult 
      @changeCurTypeGame=onChangeCurTypeGame />
    </div>
    <button class="btn-start" @click="onStartGame">Start Game</button>   
  </div>
</template>

<script>
import GameBoard from '@/components/GameBoard';
import GameOptions from '@/components/GameOptions';
export default {
  name: 'App',
  data: () => ({ 
    level: 1,
    colorArray: ["blue", "red", "green", "yellow"],
    curTimerDifficult: 1500,
    curTypeGame: "normal",
    curColorSequence: [],
    isRunning: false,
    isGameOver: false,
  }),
  components: {
    GameBoard,
    GameOptions,
  },
  methods:{
    onStartGame(){
      if(!this.isRunning){
        this.isGameOver = false;
        this.isRunning = true;
        this.onCreateSequence();
      }
    },
    onGameOver(){
      this.isRunning = false;
      this.isGameOver = true;
      this.level = 1;
      this.curColorSequence = [];
    },
    onCreateSequence(){
      this.curColorSequence = [];
      for(let i = 0; i < this.level; i++ ){
        let curColor = this.randomColor();
        this.curColorSequence.push(curColor);
      }
    },
    randomColor(){
      return this.colorArray[Math.round(Math.random() * 3)] ;
    },
    onChangeCurDifficult(timer){
      this.curTimerDifficult = timer;
    },
    onChangeCurTypeGame(type){
      this.curTypeGame = type;
    },
    onNextLevel(){
      ++this.level;
      this.onCreateSequence();
    }
  },
}
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  -webkit-user-select: none;        
	-moz-user-select: none;
	-ms-user-select: none; 
	-o-user-select: none;
	user-select: none;
}

.wrapper{
	width: 720px;
	margin: 0 auto;
}

.game-flex{
  display: flex;
  justify-content: space-around;
  align-content: center;
  flex-direction: row;
}

h1{
  margin-top: 20px;
  margin-bottom: 40px;
	text-align: center;
  font-size: 50px;
  font-weight: bold;
}

.game-over{
  font-size: 20px;
  color: red;
  margin-bottom: 20px;
  text-shadow: 0 2px 4px red;
}

.level{
  margin-bottom: 20px;
  font-size: 20px;
  font-weight: bold;
}

.btn-start{
  border-radius: 10px;
  font-size: 20px;
  font-weight: bold;
  margin-top: 20px;
  padding: 15px 35px;
  color: #fff;
  letter-spacing: 1px;
  background: dodgerblue;
  }

</style>
