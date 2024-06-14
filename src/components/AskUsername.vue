<script setup lang="ts">
  import { defineProps, defineEmits, ref } from 'vue';
  import { IGameState } from '../models/IGameState';

  const emit = defineEmits(["usernameSubmitted"]);
  const props = defineProps<{
    xoro: string;
    gameState: IGameState;
  }>();

  let name = ref("");

  const submitUserName = () => {
    if (props.xoro === "X") {
      props.gameState.users.nameX = name.value;
    } else {
      props.gameState.users.nameO = name.value;
    }
    emit("usernameSubmitted", { xoro: props.xoro, name });
  };
</script>

<template>
  <div class="welcome">
    <h1>Välkommen till Tic Tac Toe!</h1>
    <p>För att starta spelet, ange namnen för spelare X och O nedan.</p>
    <label>Namnet på spelare {{ xoro }}</label>
    <input v-model="name" class="askPlayer"/>
    <button class="usernameBtn" @click="submitUserName">Nästa</button>
  </div>
</template>

<style scoped>
  .usernameBtn {
    border-radius: 1rem;
    padding: 1rem;
    background-color: #b35d90; 
    color: #f5e6e8;
    margin: 1rem;
    border: 3px solid transparent;
    transition: border-color 0.25s;
  }

  .usernameBtn:hover {
    border-color: #ed5ebd; 
  }

  input {
    border-radius: 1rem;
    padding: 1rem;
    font-size: 1rem;
    color: #b35d90; 
    font-weight: bolder;
    border: 3px solid #b35d90; 
    background-color: #ffe6f0; 
  }

  label {
    font-size: 1.5rem;
    margin: 1rem;
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
      max-width: 30rem;
    }

    h1 {
      font-size: 2rem;
    }
  }
</style>