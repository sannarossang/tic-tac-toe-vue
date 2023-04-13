<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";

const playerList = ref<Player[]>([]);

const playerOneUserName = ref("");
const playerTwoUserName = ref("");

let emit = defineEmits(["startGame"]);

const handleSubmit = () => {
  playerList.value = [];
  if (playerOneUserName.value !== "") {
    playerList.value.push(new Player(playerOneUserName.value, "X"));
  }
  if (playerTwoUserName.value !== "") {
    playerList.value.push(new Player(playerTwoUserName.value, "O"));
  }
};

const playGame = () => {
  console.log("inside play game", playerList);

  if (playerList.value.length === 2) {
    emit("startGame", playerList.value);
  } else {
    //TODO fin validering i appen istöllet
    console.log("Det måste vara två spelare med");
  }
};
console.log("efter submit", playerList.value);
</script>

<template>
  <h3>Lets play!</h3>
  <form @submit.prevent>
    <div>
      <input v-model="playerOneUserName" type="text" placeholder="Player one" class="textbox" />
    </div>
    <div>
      <input v-model="playerTwoUserName" type="text" placeholder="Player two" class="textbox" />
    </div>
    <div>
      <button type="submit" class="btn" @click="handleSubmit">Submit</button>
    </div>
  </form>
  <p>Now, these two are up for a game of Tic Tac Toe:</p>
  <div v-for="player in playerList">{{ player.name }}</div>
  <button type="submit" class="btn" @click="() => playGame()">All set up! Let's play the game</button>
</template>

<style scoped>
h3 {
  color: pink;
}

.textbox {
  color: pink;
  border: dotted pink;
  margin: 10px;
  padding: 10px;
  font-size: 20px;
}
.btn {
  background-color: pink;
  padding: 15px;
  margin: 15px;
}

p {
  font-size: 25px;
}
</style>
