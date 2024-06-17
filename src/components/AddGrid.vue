<script setup lang="ts">
import { ref, watch } from 'vue';

const props = defineProps<{
  currentPlayerSymbol: string;
}>();

const board = ref(Array(9).fill(''));
const currentPlayer = ref(props.currentPlayerSymbol);

const handleCellClick = (index: number) => {
  if (board.value[index] !== '') return;

  board.value[index] = currentPlayer.value;
  

  currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
};

watch(() => props.currentPlayerSymbol, (newSymbol) => {
  currentPlayer.value = newSymbol;
});
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
