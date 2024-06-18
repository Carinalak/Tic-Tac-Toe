<script setup lang="ts">
import { ref, computed, watch } from 'vue';
import AddGrid from './AddGrid.vue';

const props = defineProps<{
  playerX: { name: string, symbol: string, points: number } | null;
  playerO: { name: string, symbol: string, points: number } | null;
  currentPlayerIndex: number | null;
}>();

const currentPlayerIndex = ref(props.currentPlayerIndex);
const currentPlayerSymbol = computed(() => currentPlayerIndex.value === 0 ? 'X' : 'O');
const currentPlayerName = computed(() => currentPlayerSymbol.value === 'X' ? props.playerX?.name : props.playerO?.name);
const winner = ref<string | null>(null);
const gameDraw = ref(false);

const emit = defineEmits(['playerSwitched', 'gameWon', 'gameDraw']);


const switchPlayer = () => {
  currentPlayerIndex.value = currentPlayerIndex.value === 0 ? 1 : 0;
};
const handleGameDraw = () => {
  gameDraw.value = true;
};

const handleGameWon = (symbol: string) => {
  winner.value = symbol === 'X' ? props.playerX?.name ?? null : props.playerO?.name ?? null;
  if (symbol === 'X') {
    props.playerX!.points += 1;
    localStorage.setItem('playerX', JSON.stringify(props.playerX));
  } else {
    props.playerO!.points += 1;
    localStorage.setItem('playerO', JSON.stringify(props.playerO)); 
  }
};




const resetGame = () => {
  winner.value = null;
  gameDraw.value = false;
  currentPlayerIndex.value = props.currentPlayerIndex;
};

watch(winner, (newWinner) => {
  if (newWinner) {
  }
});

watch(gameDraw, (isDraw) => {
  if (isDraw) {
    alert("Oavgjort, börja om");
  }
});

</script>

<template>
<section class="main-container"> 
 <div v-if="playerX && playerO">
    <p>Spelare X: {{ playerX.name }}, Poäng: {{ playerX.points }}</p>
    <p>Spelare O: {{ playerO.name }}, Poäng: {{ playerO.points }}</p>
    <article class="winner-container-height">
      <div class="winner-container" v-if="winner && !gameDraw">
        <h2>Grattis {{ winner }}, du vann!</h2>
        <button @click="resetGame">Spela igen</button>
      </div>
    </article>
    <div class="main-container" v-if="gameDraw">
      <p>Oavgjort, börja om</p>
      <button @click="resetGame">Spela igen</button>
    </div>
  </div>

  <div>
    <p class="main-container" v-if="!winner && !gameDraw">Det är {{ currentPlayerName }}s tur att spela.</p>
    <AddGrid :currentPlayerSymbol="currentPlayerSymbol" @playerSwitched="switchPlayer" @gameWon="handleGameWon" @gameDraw="handleGameDraw" :gameOver="!!winner || gameDraw" />
  </div> 
</section>

</template>



<style scoped>
h2 {
  color: purple;
  font-weight: bold;
}

.main-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: aqua;
}

.winner-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: aqua;
}
.winner-container-height {
  height: 90px;
}
</style>
