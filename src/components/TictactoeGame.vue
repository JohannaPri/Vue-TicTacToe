<script setup lang="ts">
import { reactive } from 'vue';
import { IGameState } from '../models/IGameState';


const gameState = reactive<IGameState>({
    gameboard: ["", "", "", "", "", "", "", "", ""],
    showScores: false,
    users: {
        nameX: "",
        nameO: "",
    },
    scores: {
        scoresX: 0,
        scoresO: 0
    },
    isXturn: false,
    gameOver: false
});

</script>

<template>
  
</template>

<style scoped>

</style>