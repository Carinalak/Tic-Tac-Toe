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

const emit = defineEmits(['playerSwitched', 'gameWon', 'gameDraw', 'resetPlayers']);

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
  currentPlayerIndex.value = Math.random() < 0.5 ? 0 : 1; 
  emit('playerSwitched');
  //currentPlayerIndex.value = props.currentPlayerIndex;
};

watch(winner, (newWinner) => {
  if (newWinner) {
    console.log(newWinner, "vann! Spela igen?");
    
  }
});

watch(gameDraw, (isDraw) => {
  if (isDraw) {
    alert("Oavgjort, börja om");
    console.log("oavgjort, börja om");
    
  }
});
</script>

<template>

  <div class="all-container">
    <section class="main-container"> 
      <div class="game-container">
        <article class="winner-container-height" v-if="winner && !gameDraw">
          <div class="winner-container">
            <h3>* * Grattis <span class="name-color">{{ winner }}</span>, du vann! * *</h3>
          </div>
          
        </article>
        <div>
          <div v-if="gameDraw"><p>Oavgjort, börja om!</p></div>
          <p v-if="!winner && !gameDraw">Det är <span class="name-color">{{ currentPlayerName }}s </span>tur att spela</p>
          <AddGrid :currentPlayerSymbol="currentPlayerSymbol" @playerSwitched="switchPlayer" @gameWon="handleGameWon" @gameDraw="handleGameDraw" :gameOver="!!winner || gameDraw" />
          <div class="winner-container" v-if="winner && !gameDraw">
            <button @click="resetGame">Spela igen</button>
          </div>
          <div v-if="gameDraw">
            <button @click="resetGame">Spela igen</button>
          </div>
        </div>
        <div class="button-wrap">
          <button @click="$emit('resetPlayers')" class="reset-button">Logga in igen</button>
        </div>
        
        <div class="score-container" v-if="playerX && playerO">
          <p><span class="name-color">{{ playerX.name }}</span> - {{ playerX.points }} poäng<br>
          <span class="name-color">{{ playerO.name }}</span> - {{ playerO.points }} poäng</p>
        </div>
      </div>
    </section>
  </div>

</template>

<style scoped>
h3 {
  color: purple;
  font-weight: bold;
}

.main-container {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    color: purple;
}

.winner-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.score-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
    font-weight: bold;
    height: 90px;
    width: 300px;
    border: 1px solid purple;
    background-color: rgb(206, 171, 239);
    border-radius: 5px;
    margin-top: 5px;
    margin-bottom: 20px;
}
.game-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
}
.all-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.name-color {
  color: rgb(228, 29, 181);
}

</style>
