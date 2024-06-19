<script setup lang="ts">
  import { IGameState } from '../models/IGameState';
  import { defineEmits, defineProps } from 'vue';

  // Definiera emit-händelser för gameRestarted och newGameStarted
  const emit = defineEmits<{
    (e: 'gameRestarted'): void;
    (e: 'newGameStarted'): void;
  }>();

  // Definiera props med typ för gameState från IGameState
  const props = defineProps<{
    gameState: IGameState;
  }>();

  // Funktion för att starta om spelet
  const restartGame = () => {
    props.gameState.gameboard = ["", "", "", "", "", "", "", "", ""];
    props.gameState.gameOver = false;
    emit("gameRestarted");
  };

  // Funktion för att starta ett nytt spel med nya spelare
  const newGame = () => {
    props.gameState.gameboard = ["", "", "", "", "", "", "", "", ""];
    props.gameState.gameOver = false;
    props.gameState.users.nameX = "";
    props.gameState.users.nameO = "";
    props.gameState.scores.scoresX = 0;
    props.gameState.scores.scoresO = 0;
    localStorage.removeItem("gameState");
    emit("newGameStarted");
  };
</script>

<template>
  <!-- Knappar för att starta om spel eller starta nytt spel -->
  <div class="commandButtons">
    <div class="restartGame" v-if="gameState.gameOver">
      <button @click="restartGame">Starta om</button>
      <button @click="newGame">Spela med nya spelare</button>
    </div>
  </div>
</template>

<style scoped>
  .commandButtons {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .restartGame {
    margin: 1rem;
    position: absolute;
    margin-top: 3rem;
  }

  button {
    margin: 1rem;
    background-color: #b35d90; 
    color: #ffe6f0; 
    border-radius: 8px;
    border: 3px solid transparent;
    padding: 0.6em 1.2em;
    font-size: 1rem;
    font-weight: 500;
    font-family: inherit;
    cursor: pointer;
    transition: border-color 0.25s;
    letter-spacing: 0.03rem;
  }

  button:hover {
    border-color: #ed5ebd; 
  }

  button:focus,
  button:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
  }
</style>