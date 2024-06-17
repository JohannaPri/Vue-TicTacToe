<script setup lang="ts">
    import { watch, reactive, defineEmits } from 'vue';
    import GameBoard from './GameBoard.vue';
    import CommandButtons from './CommandButtons.vue';
    import AskUsername from './AskUsername.vue';
    import { IGameState } from '../models/IGameState';
    import ShowScores from './ShowScores.vue';

    // Definierar en 'play'-emit med ett index
    const emit = defineEmits<{
        (e: 'play', index: number): void;
    }>();

    // Skapa ett reaktivt gameState-objekt med spelens aktuella tillstånd
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
    // Bevaka förändringar i gameState och spara till localStorage
    watch(gameState, (newGameState) => {
        localStorage.setItem("gameState", JSON.stringify(newGameState));
    });

    // Funktion för att spela spelet när en spelruta klickas på
    const playGame = (index: number) => {
    emit('play', index);
};
</script>

<template>
    <!-- Rendera hela Tic Tac Toe-spelet -->
    <div class="ticTacToeGame">
        <!-- Visa spelbrädet och scores om båda spelarna har angett sina namn -->
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
        box-sizing: border-box;
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