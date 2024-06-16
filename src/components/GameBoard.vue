<script setup lang="ts">
  import { defineProps, defineEmits } from 'vue';
  import SingleSquare from './SingleSquare.vue';
  import { IGameState } from '../models/IGameState';

  // Definiera props med typ för gameState från IGameState
  const props = defineProps<{
    gameState: IGameState;
  }>();

  // Definiera emit-händelsen för 'play'
  const emit = defineEmits(["play"]);

  let currentUser = props.gameState.users.nameX;

  // Funktion för att hantera ett speldrag
  const play = (index: number) => {
    // Om spelet är över, returnera
    if (props.gameState.gameOver) {
      return;
    }

    const { gameboard } = props.gameState;
    if (gameboard[index].length === 0) {
      // Uppdatera spelbrädet beroende på vilken tur det är
      if (props.gameState.isXturn) {
        gameboard[index] = "X";
        currentUser = props.gameState.users.nameO;
      } else {
        gameboard[index] = "O";
        currentUser = props.gameState.users.nameX;
      }
      // Växla spelarens tur
      props.gameState.isXturn = !props.gameState.isXturn;

      // Kontrollera om spelet är över
      checkIfGameOver(index);
      // Emit händelse när ett drag har gjorts
      emit("play", { index, player: gameboard[index] });
    }
  };

  function checkIfGameOver(index: number) {
     // Definiera alla vinnande kombinationer
    const winningCombinations = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8], 
      [0, 3, 6], [1, 4, 7], [2, 5, 8], 
      [0, 4, 8], [2, 4, 6] 
    ];

    const currentPlayer = props.gameState.gameboard[index];

    // Kontrollera om det finns en vinnande kombination
    for (const combination of winningCombinations) {
      const [a, b, c] = combination;
      if (
        props.gameState.gameboard[a] === currentPlayer &&
        props.gameState.gameboard[b] === currentPlayer &&
        props.gameState.gameboard[c] === currentPlayer
      ) {
        // Uppdatera poängen beroende på vinnaren
        if (currentPlayer == "O") {
          props.gameState.scores.scoresO++;
        } else {
          props.gameState.scores.scoresX++;
        }
        // Markera spelet som över
        props.gameState.gameOver = true;
        break;
      }
    }

    // Kontrollera om det är oavgjort
    const isTie = props.gameState.gameboard.every((square) => square !== "");
    if (isTie) {
      props.gameState.gameOver = true;
    }
  }
</script>

<template>
  <!-- Visa vems tur det är om spelet inte är över, annars visa 'Game Over' -->
  <h1 class="whosTurn" v-if="!gameState.gameOver">Det är {{ currentUser }}s tur!</h1>
  <h1 class="gameOver" v-else>Game Over!</h1>
  <!-- Visa spelbrädet -->
  <div class="gameBoard">
    <SingleSquare
      v-for="(square, index) in gameState.gameboard"
      :key="index"
      :state="square"
      :clickable="!gameState.gameOver && square === ''"
      @click="() => play(index)"
    />
  </div>
</template>

<style scoped>
  h1 {
    color: #b35d90;
     letter-spacing: 0.05rem;
  }

  .whosTurn {
    margin-top: 15px;
    letter-spacing: 0.1rem;
  }

  .gameOver {
    text-transform: uppercase;
    letter-spacing: 0.1rem;
    animation: gameOver 2s infinite;
    margin-top: 15px;

  }

  @keyframes gameOver {
    0%, 100% {
      transform: scale(1); 
    }
    50% {
      transform: scale(1.1); 
    }
  }
  
  .gameBoard {
    width: 23rem;
    height: 23rem;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 0px;
    background-color: #ffccd5; 
    color: #b35d90; 
  }

  @media (max-width: 740px) {
    .gameBoard {
      width: 20rem;
      height: 20rem;
    }

    h1 {
      font-size: 2rem;
    }
  }
</style>
