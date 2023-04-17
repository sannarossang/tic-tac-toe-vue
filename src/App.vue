<script setup lang="ts">
import { ref } from "vue";
import SetupPlayers from "./components/SetupPlayers.vue";
import Game from "./components/Game.vue";
import { Player } from "./models/Player";

let startGame = ref<boolean>(false);
let playerList = ref<Player[]>([]);

function toggleGame(players: Player[]) {
  playerList.value = players;
  startGame.value = true;
}

function quitGame() {
  startGame.value = false;
  playerList.value = [];
}
</script>

<template>
  <h1>Tic Tac Toe by Rossäng</h1>
  <SetupPlayers v-if="startGame === false" @start-game="toggleGame" />
  <Game v-else @quit-game="quitGame" :players="playerList" />
</template>

<style scoped></style>
