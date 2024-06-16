<script setup lang="ts">
import { ref } from 'vue';

const grid = ref(Array.from({ length: 9 }, (_, index) => index));
/*
const handleCellClick = (index: number) => {
  console.log(`Ruta ${index + 1} klickad`);
  
};
*/
// Initialisera en tom spelplan med 9 celler
const board = ref(Array(9).fill(''));

// Spelare X börjar
const currentPlayer = ref('X');

// Uppdatera aktuell spelare vid spelstart
/*
const setCurrentPlayer = () => {
    if (currentPlayerIndex.value !== null) {
        currentPlayer.value = currentPlayerIndex.value === 0 ? 'X' : 'O';
    }
};
*/

const handleCellClick = (index: number) => {
  // Kontrollera om cellen redan är upptagen
  if (board.value[index] !== '') return;

  // Sätt spelarens symbol i cellen
  board.value[index] = currentPlayer.value;

  // Växla spelare
  currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
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
  
<!--
    <div class="grid-container">
      <div 
        v-for="index in grid" 
        :key="index" 
        class="grid-cell" 
        @click="handleCellClick(index)"
      >
        {{ index + 1 }}
      </div>
    </div>-->
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
  