<script setup lang="ts">
    import { watch, reactive, defineEmits } from 'vue';
    import GameBoard from './GameBoard.vue';
    import CommandButtons from './CommandButtons.vue';
    import AskUsername from './AskUsername.vue';
    import { IGameState } from '../models/IGameState';
    import ShowScores from './ShowScores.vue';

    const emit = defineEmits(["play"]);

    const gameState = reactive<IGameState>({
    gameboard: ["", "", "", "", "", "", "", "", ""],
    showScores: false,
    users: {
        nameX: "",
        nameO: "",
    },
    scores: {
        scoresX: 0,
        scoresO: 0,
    },
    isXturn: true,
    gameOver: false,
    });

    // Hämta gameState från localStorage om det finns
    const gameStateFromLocalStorage = localStorage.getItem("gameState");
    if (gameStateFromLocalStorage) {
    Object.assign(gameState, JSON.parse(gameStateFromLocalStorage));
    }

    // Spara gameState till localStorage när det ändras
    watch(gameState, (newGameState) => {
    localStorage.setItem("gameState", JSON.stringify(newGameState));
    });

    const playGame = (index: number) => {
    // Exekvera logik för att spela spelet och uppdatera gameState vid behov
    emit("play", index);
    };
</script>

<template>
    <div class="ticTacToeGame">
        <div class="gameBoardShow" v-if="gameState.users.nameX.length > 0 && gameState.users.nameO.length > 0">
            <GameBoard :gameState="gameState" @play="playGame" />
        </div>
        <div class="aside" v-if="gameState.users.nameX.length > 0 && gameState.users.nameO.length > 0">
            <ShowScores :gameState="gameState" />
        </div>
        <div>
            <CommandButtons :gameState="gameState" />
        </div>
            <div v-if="gameState.users.nameX.length === 0">
            <AskUsername :xoro="'X'" :gameState="gameState" />
        </div>
            <div v-else-if="gameState.users.nameO.length === 0">
            <AskUsername :xoro="'O'" :gameState="gameState" />
        </div>
    </div>
</template>

<style scoped>
    .ticTacToeGame {
        background-color: #ffccd5; 
        border: 5px solid #b35d90; 
        padding: 2rem;
        border-radius: 10px;
        height: fit-content;
    }

    .gameBoardShow {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .aside {
        position: relative;
        display: contents;
        border: 5px solid #b35d90; 
        background-color: #ffccd5; 
        color: #b35d90; 
        right: 0;
        top: 0;
        border-radius: 10px;
        margin-right: 2rem;
        margin-top: 2rem;
        padding: 2rem;
    }
</style>