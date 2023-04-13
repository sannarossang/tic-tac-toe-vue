<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";
import { Square } from "../models/Square";

// const currentPlayer = ref(0);
const props = defineProps<IGameBoardProps>();

const currentPlayer = ref<Player>(props.players[0]);

// // const currentPlayer = ref<Player>(props.players[0]);

interface IGameBoardProps {
  players: Player[];
}

const squares = ref<Square[]>([
  new Square("", false, ""),
  new Square("", false, ""),
  new Square("", false, ""),
  new Square("", false, ""),
  new Square("", false, ""),
  new Square("", false, ""),
  new Square("", false, ""),
  new Square("", false, ""),
  new Square("", false, ""),
]);

const toggleSquare = (i: number) => {
  squares.value[i].clicked = !squares.value[i].clicked;
  squares.value[i].marker = currentPlayer.value.marker;

  // if (currentPlayer === props.players[0]) {
  //   currentPlayer === props.players[1];
  // } else {
  //   currentPlayer === props.user[0];
  // }

  //om spelar ett klickar ge ett kryss
  //om spelare två klickar ge en cirkel
};
</script>

<template>
  <div class="board">
    <div
      v-for="(square, index) in squares"
      :key="index"
      class="square"
      :class="square.clicked ? 'clicked' : ''"
      @click.once="toggleSquare(index)"
    >
      {{ square.marker }}
      <!-- //det som syns i rutan -->
    </div>
  </div>
  <h3>These little fellas is playing tha game</h3>
  <p>Now is it your turn: {{ currentPlayer.name }}</p>

  <!-- <button type="button" @click.once="playGame">Starta spelet</button> -->
</template>

<style scoped>
.board {
  width: 600px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}
.square {
  background: pink;
  border: 1px dotted purple;
  height: 150px;
  width: 150px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.clicked {
  background-color: blue;
}
</style>
