<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";
import { Square } from "../models/Square";

const props = defineProps<IGameBoardProps>();

const currentPlayer = ref<Player>(props.players[0]);

let winner = ref<Player>();
const gameOver = ref<boolean>(false);

const reset = () => {
  board.value = [
    new Square(false, ""),
    new Square(false, ""),
    new Square(false, ""),
    new Square(false, ""),
    new Square(false, ""),
    new Square(false, ""),
    new Square(false, ""),
    new Square(false, ""),
    new Square(false, ""),
  ];
  gameOver.value = false;
  winner.value = new Player("", "");
  if (currentPlayer.value === props.players[0]) {
    currentPlayer.value = props.players[1];
  } else {
    currentPlayer.value = props.players[0];
  }
};

interface IGameBoardProps {
  players: Player[];
}

let emit = defineEmits(["quitGame"]);
function quitGame() {
  emit("quitGame");
}

const board = ref<Square[]>([
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

const toggleSquare = (i: number) => {
  if (gameOver.value || board.value[i].clicked) {
    return;
  }

  board.value[i].clicked = !board.value[i].clicked;
  board.value[i].marker = currentPlayer.value.marker;
  isWinner();
  if (gameOver.value === false) {
    if (currentPlayer.value === props.players[0]) {
      currentPlayer.value = props.players[1];
    } else {
      currentPlayer.value = props.players[0];
    }
  }
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

      let potentialMarkerValue = board.value[positionToGetValueFrom].marker;
      potentialWinningLineMarkers.push(potentialMarkerValue);

      const xIsWinner = (currentValue: string) => currentValue == "X";
      const oIsWinner = (currentValue: string) => currentValue == "O";

      if (potentialWinningLineMarkers.every(xIsWinner) && potentialWinningLineMarkers.length == 3) {
        gameOver.value = true;
        winner.value = currentPlayer.value;
      }

      if (potentialWinningLineMarkers.every(oIsWinner) && potentialWinningLineMarkers.length == 3) {
        gameOver.value = true;
        winner.value = currentPlayer.value;
      }
    }
  }
}
</script>

<template>
  <h3>These little fellas is playing tha game:</h3>
  <h3>{{ props.players[0].name }} and {{ props.players[1].name }}</h3>
  <p v-if="winner == null">And now it is your turn: {{ currentPlayer.name }}</p>
  <p v-else>We have a winner: {{ winner?.name }}</p>
  <p></p>

  <div class="board">
    <div
      v-for="(square, index) in board"
      :key="index"
      class="square"
      :class="square.clicked ? 'clicked' : ''"
      @click="toggleSquare(index)"
    >
      {{ square.marker }}
    </div>
  </div>

  <button type="button" @click="reset()">Play again</button>
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
