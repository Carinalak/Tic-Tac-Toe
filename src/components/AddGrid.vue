<script setup lang="ts">
import {ref, watch } from 'vue';

const props = defineProps<{
  currentPlayerSymbol: string;
  gameOver: boolean;
}>();


const emit = defineEmits(['playerSwitched', 'gameWon']);

const board = ref(Array(9).fill(''));
//const gameOver = ref(false);

const winningCombinations = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8], // rows
  [0, 3, 6], [1, 4, 7], [2, 5, 8], // columns
  [0, 4, 8], [2, 4, 6]             // diagonals
];


const handleCellClick = (index: number) => {
  if (board.value[index] !== '' || props.gameOver) return;

  board.value[index] = props.currentPlayerSymbol;

  if (checkWinner(props.currentPlayerSymbol)) {
    //gameOver.value = true;
    emit('gameWon', props.currentPlayerSymbol);
  } else {
    emit('playerSwitched');
  }
};

const checkWinner = (symbol: string) => {
  return winningCombinations.some(combination => {
    return combination.every(index => board.value[index] === symbol);
  });
};

watch(() => props.gameOver, (newGameOver) => {
  if (!newGameOver) {
    resetBoard();
  }
});

const resetBoard = () => {
  board.value = Array(9).fill('');
};

</script>

<template>
  <div class="grid-container">
    <div 
      v-for="(cell, index) in board" 
      :key="index" 
      class="grid-cell" 
      @click="handleCellClick(index)"
    >
      {{ cell }}
    </div>
  </div>
</template>

<style scoped>
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 5px;
  width: 300px;
  margin-top: 10px;
}

.grid-cell {
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #000;
  height: 100px;
  font-size: 24px;
  cursor: pointer;
}
</style>
