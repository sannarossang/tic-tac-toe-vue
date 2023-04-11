<script setup lang="ts">
import { ref } from "vue";
import { Player } from "../models/Player";

// const state = ref<Player>({ name: "Sanna", id: 2 });

const handleClick = (num: number, event: MouseEvent) => {
  console.log("Du klickade på", num, event);
};
</script>

<template>
  <div>Tic Tac Toe by Rossäng</div>
  <div class="board">
    <div class="square" v-for="num of 9" :key="num" @click="handleClick(num, $event)">
      {{ num }}
    </div>
  </div>
</template>

<style scoped>
.board {
  width: 500px;
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
</style>
