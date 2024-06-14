<script setup lang="ts">
import { ref } from 'vue';
import { Player } from './models/Player';


const playerText = ref("");
const playerX = ref<Player | null>(null);
const playerO = ref<Player | null>(null);
const isPlayerX = ref(true);
const buttonText = ref("Spara");

const handleSubmit = () => {
    if (isPlayerX.value) {
        // Skapar en ny spelare X
        playerX.value = new Player(playerText.value, 'X');
        console.log(`Spelare X sparad: ${playerX.value.name}`);
        playerText.value = "";
        isPlayerX.value = false;  // Växlar till spelare O
    } else {
        // Skapar en ny spelare O
        playerO.value = new Player(playerText.value, 'O');
        console.log(`Spelare O sparad: ${playerO.value.name}`);
        playerText.value = "";

        buttonText.value = "Börja spela";

        // Väljer slumpmässigt vilken spelare som ska börja
        //currentPlayerIndex.value = Math.random() < 0.5 ? 0 : 1;
        //console.log(`Spelare ${currentPlayerIndex.value === 0 ? 'X' : 'O'} börjar spela`);
    }
}

</script>
<template>

<form @submit.prevent="handleSubmit">
    <label>{{ isPlayerX ? 'Spelare X' : 'Spelare O' }}: </label>
    <input type="text" v-model="playerText" />
    <button @click="handleSubmit"> {{ buttonText }}</button>
</form>








</template>
<style scoped>

</style>
