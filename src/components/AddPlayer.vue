<script setup lang="ts">
import { ref } from 'vue';
import { Player } from './models/Player';
import AddGrid from './AddGrid.vue';

const playerText = ref("");
const playerX = ref<Player | null>(null);
const playerO = ref<Player | null>(null);
const isPlayerX = ref(true);
const buttonText = ref("Spara");
const currentPlayerIndex = ref<number | null>(null);
const gameStarted = ref(false);

const handleSubmit = () => {
    if (isPlayerX.value) {
        playerX.value = new Player(playerText.value, 'X');
        console.log(`Spelare X sparad: ${playerX.value.name}`);
        playerText.value = "";
        buttonText.value = "Börja spela";
        isPlayerX.value = false; 
    } else {
        playerO.value = new Player(playerText.value, 'O');
        console.log(`Spelare O sparad: ${playerO.value.name}`);
        playerText.value = "";
        startGame();
    }  
};

const startGame = () => {
    currentPlayerIndex.value = Math.random() < 0.5 ? 0 : 1;
    gameStarted.value = true;
    console.log(`Spelet har startat. Spelare ${currentPlayerIndex.value === 0 ? 'X' : 'O'} börjar spela`);
}

const currentPlayer = () => {
    if (currentPlayerIndex.value !== null) {
        return currentPlayerIndex.value === 0 ? playerX.value?.name : playerO.value?.name;
    } else {
        return '';
    }
};
</script>
<template>
<div v-if="!gameStarted">
    <form @submit.prevent="handleSubmit">
        <label>{{ isPlayerX ? 'Spelare X' : 'Spelare O' }}: </label>
        <input type="text" v-model="playerText" />
        <button type="submit"> {{ buttonText }}</button>
    </form>
</div>

<div v-else>
    Det är {{ currentPlayer() }}s tur att spela.
    <AddGrid :gridSize="9" />
</div>


</template>
<style scoped>


</style>
