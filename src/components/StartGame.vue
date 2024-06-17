<script setup lang="ts">
import { defineProps, ref, watch } from 'vue';
import AddGrid from './AddGrid.vue';

const props = defineProps<{
  playerX: { name: string, symbol: string } | null;
  playerO: { name: string, symbol: string } | null;
  currentPlayerIndex: number | null;
}>();

const currentPlayerSymbol = ref(props.currentPlayerIndex === 0 ? 'X' : 'O');

watch(() => props.currentPlayerIndex, (newIndex) => {
  currentPlayerSymbol.value = newIndex === 0 ? 'X' : 'O';
});

const getCurrentPlayer = () => {
  if (props.currentPlayerIndex === null) return '';
  return props.currentPlayerIndex === 0 ? props.playerX?.name : props.playerO?.name;
};
</script>

<template>
  <div v-if="playerX && playerO">
    <p>
      Spelare X: {{ playerX.name }}, Spelare O: {{ playerO.name }}
    </p>
  </div>

  <div>
    Det är {{ getCurrentPlayer() }}s tur att spela.
    <AddGrid :currentPlayerSymbol="currentPlayerSymbol" />
  </div>
</template>

<style scoped>

</style>
