<script setup lang="ts">
import { ref } from "vue";
import TicTac from "./components/TicTac.vue";
import SetupPlayers from "./components/SetupPlayers.vue";
import GameBoard from "./components/GameBoard.vue";
import { Player } from "./models/Player";

let startGame = ref<boolean>(false);
let playerList = ref<Player[]>([]);

function toggleGame(players: Player[]) {
  console.log("toggeld");
  playerList.value = players;

  startGame.value = true;
}
</script>

<template>
  <h1>Tic Tac Toe by Rossäng</h1>
  <SetupPlayers v-if="startGame === false" @start-game="toggleGame" />
  <GameBoard v-else :players="playerList" />
</template>

<style scoped></style>
