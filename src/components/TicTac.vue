<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";
import GameBoard from "./GameBoard.vue";
import SetupPlayers from "./SetupPlayers.vue";

// <div class="players" v-if="showPlayers"></div>

let currentTurn = "X";

let gameOver = ref(true);

function clickedSquare(index) {
  if (squares.value[index] !== null) return;

  squares.value[index] = currentTurn;
  currentTurn === "X" ? (currentTurn = "O") : (currentTurn = "X");
  return { clickedSquare };
}

function isWinner() {
  const lines = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
    [1, 4, 7],
    [2, 5, 8],
    [3, 6, 9],
    [1, 5, 9],
    [3, 5, 7],
  ];
}
</script>

<template>
  <GameBoard />
  <SetupPlayers />
</template>

<style scoped></style>
