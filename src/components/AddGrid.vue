<script setup lang="ts">
import { ref } from 'vue';



const grid = ref(Array.from({ length: 9 }, (_, index) => index));

const handleCellClick = (index: number) => {
  console.log(`Ruta ${index + 1} klickad`);
};


</script>
<template>
    <div class="grid-container">
      <div 
        v-for="index in grid" 
        :key="index" 
        class="grid-cell" 
        @click="handleCellClick(index)"
      >
        {{ index + 1 }}
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
  