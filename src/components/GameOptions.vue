<template>
  <div class="game-options">
    <h2>Game options:</h2>
    <form @change="changeTypeGame">
      <input id="normal" type="radio" name= "options" value="normal" checked>
      <label for="normal">Standard</label><br>
      <input type="radio" name= "options" value="sound">Sound only<br>
      <input type="radio" name= "options" value="light">Light only<br>
    </form>

    <h2>Game difficulty:</h2>
    <form @change="changeDifficult">
      <input type="radio" name="difficulty" value="easy" checked>Easy<br>
      <input type="radio" name="difficulty" value="medium">Medium<br>
      <input type="radio" name="difficulty" value="hard">Hard<br>
    </form>
  </div>
</template>

<script>
export default {
  name: 'GameOptions',
  data: () => ({
    difficults: {
      'easy': 1500,
      'medium': 1000,
      'hard': 400,
    },
  }),
  methods:{
    changeDifficult(event){
      event.preventDefault();
      let timerDifficult = this.difficults[event.target.value];
      this.$emit("changeCurDifficult", timerDifficult);
    },
    changeTypeGame(event){
      event.preventDefault();
      let typeGame = event.target.value;
      this.$emit("changeCurTypeGame", typeGame);
    },
  }
}
</script>

<style scoped>

.game-options h2 {
	margin-top: 30px;
  margin-bottom: 20px;
}

form {
  text-align: left;
  font-size: 20px;
}

.game-options input[type="radio"] {
  text-align: left;
	margin-right: 10px;
  margin-bottom: 15px;
}


</style>
