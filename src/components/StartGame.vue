<script setup lang="ts">
import { defineProps, ref, computed } from 'vue';
import AddGrid from './AddGrid.vue';

const props = defineProps<{
  playerX: { name: string, symbol: string } | null;
  playerO: { name: string, symbol: string } | null;
  currentPlayerIndex: number | null;
}>();

const currentPlayerSymbol = ref(props.currentPlayerIndex === 0 ? 'X' : 'O');
const currentPlayerName = computed(() => {
  return currentPlayerSymbol.value === 'X' ? props.playerX?.name : props.playerO?.name;
});
const nextPlayerName = computed(() => {
  return currentPlayerSymbol.value === 'X' ? props.playerO?.name : props.playerX?.name;
});

const winner = ref<string | null>(null);

const switchPlayer = () => {
  currentPlayerSymbol.value = currentPlayerSymbol.value === 'X' ? 'O' : 'X';
};

const handleGameWon = (symbol: string) => {
  winner.value = symbol === 'X' ? props.playerX?.name : props.playerO?.name;
};
</script>

<template>
 <div v-if="playerX && playerO">
    <p>Spelare X: {{ playerX.name }}, Spelare O: {{ playerO.name }}</p>
  </div>
  <div v-if="winner">
      <p>Grattis, {{ winner }} vann!</p>
    </div>
  <div v-else>
    <p>Det är {{ currentPlayerName }}s tur att spela.</p>
    <AddGrid :currentPlayerSymbol="currentPlayerSymbol" @playerSwitched="switchPlayer" @gameWon="handleGameWon" />
  </div>
</template>

<style scoped>

</style>
