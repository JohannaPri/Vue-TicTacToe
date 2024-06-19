<script setup lang="ts">
  import { defineProps, defineEmits, watch } from "vue";
  import SingleSquare from "./SingleSquare.vue";
  import { IGameState } from "../models/IGameState";

  // Definiera props för komponenten med typ för gameState
  const props = defineProps<{
    gameState: IGameState;
  }>();

  // Definiera emit-händelsen för 'play'
  const emit = defineEmits(["play"]);

  // Sätt aktuell användare baserat på vems tur det är
  let currentUser = props.gameState.isXturn ? props.gameState.users.nameX : props.gameState.users.nameO;

  // Bevaka förändringar i isXturn och uppdatera aktuell användare
  watch(() => props.gameState.isXturn, (newVal) => {
    currentUser = newVal ? props.gameState.users.nameX : props.gameState.users.nameO;
  });

  // Funktion för att hantera ett drag
  const play = (index: number) => {
    if (props.gameState.gameOver) {
      return;
    }

    const { gameboard } = props.gameState;
    if (gameboard[index].length === 0) {
      // Sätt 'X' eller 'O' baserat på vems tur det är
      if (props.gameState.isXturn) {
        gameboard[index] = "X";
      } else {
        gameboard[index] = "O";
      }
      props.gameState.isXturn = !props.gameState.isXturn;

      // Kontrollera om spelet är över
      checkIfGameOver(index);
      emit("play", { index, player: gameboard[index] });
    }
  };

  // Funktion för att kontrollera om spelet är över
  function checkIfGameOver(index: number) {
    const winningCombinations = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6] 
    ];

    const currentPlayer = props.gameState.gameboard[index];
    // Kontrollera alla vinnande kombinationer
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
    font-size: 3rem;
    
  }

  .gameOver {
    text-transform: uppercase;
    letter-spacing: 0.2rem;
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
    grid-column-gap: 2px;
    grid-row-gap: 2px;
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
      padding-bottom: 5px;
    }

    .gameOver {
      font-size: 2.5rem;
    }

    .whosTurn {
    letter-spacing: 0.1rem;
    font-size: 2rem;
  }
}
</style>