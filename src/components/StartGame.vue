<script setup lang="ts">

import { DefineProps } from 'vue';
import AddGrid from './AddGrid.vue';


const props = defineProps<{
  playerX: { name: string, symbol: string } | null;
  playerO: { name: string, symbol: string } | null;
  currentPlayerIndex: number | null;
}>();


const getCurrentPlayer = () => {
  if (props.currentPlayerIndex === null) return '';
  return props.currentPlayerIndex === 0 ? props.playerX?.name : props.playerO?.name;
};


</script>
<template>
    <div v-if="playerX || playerO">
      <p v-if="playerX && playerO">
        Spelare X: {{ playerX.name }}, Spelare O: {{ playerO.name }}
      </p>
    </div>

  <div>
    Det är {{ getCurrentPlayer() }}s tur att spela.
    <AddGrid :gridSize="9" />
  </div>





</template>
<style scoped>

</style>
