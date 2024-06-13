<script setup lang="ts">
import { ref } from 'vue';

const playerText = ref("");

const handleSubmit = () => {
    const formattedText = `Spelare X: ${playerText.value}`;
    console.log("Submit", formattedText);
    playerText.value = "";

}


</script>
<template>

<form @submit.prevent="handleSubmit">
    <label>Spelare X: </label>
    <input type="text" v-model="playerText" />
    <button @click="handleSubmit"> Spara</button>
</form>








</template>
<style scoped>

</style>
