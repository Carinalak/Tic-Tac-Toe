<script setup lang="ts">
const props = defineProps<{
  playerX: { name: string, symbol: string, points: number } | null;
  playerO: { name: string, symbol: string, points: number } | null;
}>();
</script>

<template>
  <div v-if="playerX || playerO">
    <h2>Poäng</h2>
    <p>Name Score</p>
    <p v-if="playerX">{{ playerX.name }}: {{ playerX.points }}</p>
    <p v-if="playerO">{{ playerO.name }}: {{ playerO.points }}</p>
    <button @click="$emit('backToGame')">Tillbaka till spelet</button>
  </div>
  <div v-else>
    <p>Inga poäng att visa ännu.</p>
    <button @click="$emit('backToGame')">Tillbaka till spelet</button>
  </div>
</template>

<style scoped>
h2 {
  color: purple;
  font-weight: bold;
}
</style>
