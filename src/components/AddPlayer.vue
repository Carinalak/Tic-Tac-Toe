<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { Player } from './models/Player';
import StartGame from './StartGame.vue';

const playerText = ref("");
const playerX = ref<Player | null>(null);
const playerO = ref<Player | null>(null);
const isPlayerX = ref(true);
const buttonText = ref("Spara");
const currentPlayerIndex = ref<number | null>(null);
const gameStarted = ref(false);

const handleSubmit = () => {
    if (isPlayerX.value) {
        playerX.value = new Player(playerText.value, 'X', 0);
        localStorage.setItem('playerX', JSON.stringify(playerX.value));
        console.log(`Spelare X sparad: ${playerX.value.name}`);
        playerText.value = "";
        buttonText.value = "Börja spela";
        isPlayerX.value = false;
    } else {
        playerO.value = new Player(playerText.value, 'O', 0);
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

const startGame = () => {
    currentPlayerIndex.value = Math.random() < 0.5 ? 0 : 1;
    gameStarted.value = true;
    console.log(`Spelet har startat. Spelare ${currentPlayerIndex.value === 0 ? 'X' : 'O'} börjar spela`);
}
</script>

<template>
    <div v-if="!gameStarted">
        <form class="input-form " @submit.prevent="handleSubmit">
            <label class="input-text">{{ isPlayerX ? 'Spelare X' : 'Spelare O' }}: </label>
            <input type="text" placeholder="Namn" v-model="playerText" />
            <button class="submit-btn" type="submit"> {{ buttonText }}</button>
        </form>
    </div>

    <div class="button-wrap" v-else>
        <StartGame :playerX="playerX" :playerO="playerO" :currentPlayerIndex="currentPlayerIndex" @resetPlayers="resetPlayers"/>
    </div>
</template>

<style scoped>
.submit-btn {
    margin-left: 14px;
}
input:focus {
    border: 1px solid rgb(228, 29, 181);
}
.input-text {
    color: purple;
    font-size: 2rem;
    font-weight: bold;
    margin-right: 10px;
}
.button-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
.input-form {
    margin-top: 150px;
}

@media (max-width: 600px) {
    .input-form {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 10px;
    }
    .submit-btn {
        margin-left: 0;
        margin-top: 10px;
    }
    input {
        margin-top: 15px;
    }
} 
</style>
