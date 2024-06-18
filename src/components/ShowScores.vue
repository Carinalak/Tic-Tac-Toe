

<script setup lang="ts">


const props = defineProps<{
playerX: { name: string, symbol: string, points: number } | null;
playerO: { name: string, symbol: string, points: number } | null;
}>();



</script>
<template>

<div>
    <h2>Poäng</h2>
    <p>Name Score</p>
    <p>{{ playerX.name }}: {{ playerX.points }}</p>
    <p>{{ playerO.name }}: {{ playerO.points }}</p>

    <button @click="$emit('backToGame')">Tillbaka till spelet</button>
</div>

</template>
<style scoped>

</style>

  