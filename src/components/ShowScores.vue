<script setup lang="ts">
  import { IGameState } from '../models/IGameState';
  import { defineProps } from 'vue';

  // Definiera props med typ för gameState från IGameState
  const props = defineProps<{
    gameState: IGameState;
  }>();
</script>

<template>
  <!-- Visa aktuell poängställning för spelarna -->
  <aside class="scoreShow">
    <h1 class="scoreHeading">Aktuell Poängställning</h1>
    <p class="scorePlayer">{{ props.gameState.users.nameO }}: {{ props.gameState.scores.scoresO }} poäng</p>
    <p class="scorePlayer"> {{ props.gameState.users.nameX }}: {{ props.gameState.scores.scoresX }} poäng</p>
  </aside>
</template>

<style scoped>
  .scoreShow {
    background-color: rgba(244, 185, 204, 0.5);
    padding: 8px;
    margin-top: 45px;
    padding-left: 20px;
    padding-right: 20px;
    border-radius: 8px;
    border: 2px solid #b35d90;
    letter-spacing: 0.05rem;
  }

  .scoreShow:hover {
    background-color: #ffccd5;
  }

  .scoreHeading {
    font-size: 2.5rem;
    letter-spacing: 0.1rem;
    line-height: 1.2;
  }

  .scorePlayer {
    font-size: 1.2rem;
  }

  @media (max-width: 740px) {
    .scoreHeading {
      font-size: 2rem;
    }

    .scorePlayer {
      line-height: 1.1;
    }

    .scoreShow {
      margin-top: 50px
    }
  }
</style>