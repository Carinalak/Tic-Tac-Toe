<script setup lang="ts">

import AddPlayer from './AddPlayer.vue';
import { Player } from './models/Player';
import { ref } from 'vue';
import StartGame from './StartGame.vue';

const playerX = ref<Player | null>(null);
const playerO = ref<Player | null>(null);


const updatePlayerX = (player: Player) => {
  playerX.value = player;
};

const updatePlayerO = (player: Player) => {
  playerO.value = player;
};

/*
// Funktion för att lägga till spelare och skicka data som props till AddPlayer.vue
const addPlayer = () => {
  const players: Player[] = [
    playerX.value!,
    playerO.value!
  ];
  return { players };
};
*/

</script>

<template>
  <!-- Skicka props och callback-metoder till AddPlayer.vue -->
  <AddPlayer :player-x="playerX" :player-o="playerO" @update-player-x="updatePlayerX" @update-player-o="updatePlayerO" />
</template>
<style>


</style>