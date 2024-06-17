<script setup lang="ts">
import { ref } from 'vue';
import { Player } from './models/Player';
import StartGame from './StartGame.vue';
import { onMounted } from 'vue';


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
        localStorage.setItem('playerX', JSON.stringify(playerX.value));
        console.log(`Spelare X sparad: ${playerX.value.name}`);
        playerText.value = "";
        buttonText.value = "Börja spela";
        isPlayerX.value = false;
    } else {
        playerO.value = new Player(playerText.value, 'O');
        localStorage.setItem('playerO', JSON.stringify(playerO.value));
        console.log(`Spelare O sparad: ${playerO.value.name}`);
        playerText.value = "";
        startGame();
    }  
};

onMounted(() => {
    const savedPlayerX = localStorage.getItem('playerX');
    const savedPlayerO = localStorage.getItem('playerO');
    if (savedPlayerX && savedPlayerO) {
        playerX.value = JSON.parse(savedPlayerX);
        playerO.value = JSON.parse(savedPlayerO);
        startGame();
    }
});

const resetPlayers = () => {
    localStorage.removeItem('playerX');
    localStorage.removeItem('playerO');

    playerX.value = null;
    playerO.value = null;
    isPlayerX.value = true;
    buttonText.value = 'Spara';
    currentPlayerIndex.value = null;
    gameStarted.value = false;
};
/*
const resetGame = () => {
    localStorage.removeItem('playerX');
    localStorage.removeItem('playerO');

    playerX.value = null;
    playerO.value = null;
    isPlayerX.value = true;
    buttonText.value = 'Spara';
    currentPlayerIndex.value = null;
    gameStarted.value = false;
};*/

const startGame = () => {
    currentPlayerIndex.value = Math.random() < 0.5 ? 0 : 1;
    gameStarted.value = true;
    
    console.log(`Spelet har startat. Spelare ${currentPlayerIndex.value === 0 ? 'X' : 'O'} börjar spela`);
}

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
        <StartGame :playerX="playerX" :playerO="playerO" :currentPlayerIndex="currentPlayerIndex" />
        <button @click="resetPlayers">Logga in på nytt</button>
    </div>

</template>
<style scoped>


</style>