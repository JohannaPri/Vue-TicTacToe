<script setup lang="ts">
  import { defineProps, defineEmits, ref } from 'vue';
  import { IGameState } from '../models/IGameState';

  // Definiera emit-händelser
  const emit = defineEmits<{
    (e: 'usernameSubmitted', payload: { xoro: string; name: string }): void;
  }>();

  // Definiera props med typer för xoro och gameState
  const props = defineProps<{
    xoro: string;
    gameState: IGameState;
  }>();

  // Reaktiv variabel för användarnamn
  const name = ref("");

  // Funktion för att skicka användarnamn
  const submitUserName = () => {
    if (props.xoro === "X") {
      props.gameState.users.nameX = name.value;
    } else {
      props.gameState.users.nameO = name.value;
    }
    // Emitterar händelse när användarnamn är inskickat
    emit("usernameSubmitted", { xoro: props.xoro, name: name.value });
  };
</script>

<template>
  <!-- Komponent för välkomsttext och inmatningsformulär -->
  <div class="welcome">
    <h1 class="welcomeHeading">Välkommen till Tic Tac Toe</h1>
    <p>För att starta spelet, ange namnen för spelare X och O nedan.</p>
    <label>Namnet på spelare {{ xoro }}</label>
    <input v-model="name" class="askPlayer"/>
    <button class="usernameBtn" @click="submitUserName">Nästa</button>
  </div>
</template>

<style scoped>
  .welcomeHeading {
    margin-top: 10px;
  }
   
  .usernameBtn {
    border-radius: 16px;
    padding: 16px;
    background-color: #b35d90; 
    color: #f5e6e8;
    margin: 16px;
    border: 3px solid transparent;
    transition: border-color 0.25s;
    font-size: 1rem;
    font-weight: 500;
    font-family: inherit;
    cursor: pointer;
    letter-spacing: 0.03rem;
  }

  .usernameBtn:hover {
    border-color: #ed5ebd; 
  }

  input {
    border-radius: 16px;
    padding: 16px;
    font-size: 1rem;
    color: #b35d90; 
    font-weight: bolder;
    border: 3px solid #b35d90; 
    background-color: #ffe6f0; 
  }

  label {
    font-size: 1.5rem;
    margin: 16px;
    color: #b35d90; 
  }

  p {
    line-break: auto;
  }

  .welcome {
    color: #b35d90; 
    letter-spacing: 0.03rem;
  }

  @media (max-width: 740px) {
    .welcome {
      max-width: 480px;
    }

    h1 {
      font-size: 2rem;
    }
  }
</style>