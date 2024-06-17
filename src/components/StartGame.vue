<script setup lang="ts">
import { defineProps, ref, computed } from 'vue';
import AddGrid from './AddGrid.vue';

const props = defineProps<{
  playerX: { name: string, symbol: string } | null;
  playerO: { name: string, symbol: string } | null;
  currentPlayerIndex: number | null;
}>();

const currentPlayerIndex = ref(props.currentPlayerIndex);
const currentPlayerSymbol = computed(() => currentPlayerIndex.value === 0 ? 'X' : 'O');
const currentPlayerName = computed(() => currentPlayerSymbol.value === 'X' ? props.playerX?.name : props.playerO?.name);
const winner = ref<string | null>(null);


const switchPlayer = () => {
  currentPlayerIndex.value = currentPlayerIndex.value === 0 ? 1 : 0;
};

const handleGameWon = (symbol: string) => {
  winner.value = symbol === 'X' ? props.playerX?.name : props.playerO?.name;
};


const resetGame = () => {
  winner.value = null;
  currentPlayerIndex.value = props.currentPlayerIndex;
};

</script>

<template>
 <div v-if="playerX && playerO">
    <p>Spelare X: {{ playerX.name }}, Spelare O: {{ playerO.name }}</p>
  </div>
  <div v-if="winner">
      <p>Grattis {{ winner }}, du vann!</p>
      <button @click="resetGame">Spela igen</button>
    </div>
  <div v-else>
    <p>Det är {{ currentPlayerName }}s tur att spela.</p>
    <AddGrid :currentPlayerSymbol="currentPlayerSymbol" @playerSwitched="switchPlayer" @gameWon="handleGameWon" :gameOver="!!winner" />
  </div>
</template>

<style scoped>

</style>
