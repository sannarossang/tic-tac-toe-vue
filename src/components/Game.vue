<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";
import { Square } from "../models/Square";

const props = defineProps<IGameBoardProps>();

const currentPlayer = ref<Player>(props.players[0]);

interface IGameBoardProps {
  players: Player[];
}

const squares = ref<Square[]>([
  new Square(false, ""),
  new Square(false, ""),
  new Square(false, ""),
  new Square(false, ""),
  new Square(false, ""),
  new Square(false, ""),
  new Square(false, ""),
  new Square(false, ""),
  new Square(false, ""),
]);

//byt funktionsnamn?
const toggleSquare = (i: number) => {
  squares.value[i].clicked = !squares.value[i].clicked;
  squares.value[i].marker = currentPlayer.value.marker;

  if (currentPlayer.value === props.players[0]) {
    currentPlayer.value = props.players[1];
  } else {
    currentPlayer.value = props.players[0];
  }

  isWinner();
};

function isWinner() {
  const winningLines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [2, 5, 8],
    [1, 4, 7],
    [0, 3, 6],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < winningLines.length; i++) {
    let potentialWinningCombo = winningLines[i];
    let potentialWinningLineMarkers = [];

    for (let j = 0; j < potentialWinningCombo.length; j++) {
      let positionToGetValueFrom = potentialWinningCombo[j];

      let potentialMarkerValue = squares.value[positionToGetValueFrom].marker;
      potentialWinningLineMarkers.push(potentialMarkerValue);

      const xIsWinner = (currentValue: string) => currentValue == "X";
      const oIsWinner = (currentValue: string) => currentValue == "O";

      if (potentialWinningLineMarkers.every(xIsWinner) && potentialWinningLineMarkers.length == 3) {
        console.log("X is winner");
      }

      if (potentialWinningLineMarkers.every(oIsWinner) && potentialWinningLineMarkers.length == 3) {
        console.log("O is winner");
      }
    }
  }
}

let emit = defineEmits(["quitGame"]);
function quitGame() {
  emit("quitGame");
}
</script>

<template>
  <h3>These little fellas is playing tha game</h3>
  <p>Now is it your turn: {{ currentPlayer.name }}</p>
  <p></p>

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

  <button type="button" @click.once="">Play again</button>
  <button type="button" @click.once="quitGame()">Back to start</button>
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
  background-color: purple;
}

.square {
  font-size: 55px;
  color: pink;
}
</style>
