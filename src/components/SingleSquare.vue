<script setup lang="ts">
  import { defineProps, defineEmits } from 'vue';

  // Definiera props med typen state som är en sträng
  const props = defineProps<{
    state: string;
  }>();

  // Definiera emits för händelsen "squareClicked" 
  const emit = defineEmits<{
    (e: 'squareClicked'): void;
  }>();

  // Funktion för att hantera klick på spelrutan
  const playGame = () => {
    // Kontrollera att rutan är tom innan emit skickas
    if (props.state === '') {
      emit("squareClicked");
    }
  };
</script>

<template>
  <!-- Rendera spelrutan med klick-händelse och visa värdet av state -->
  <div class="gameSquare" @click="playGame">
    <p class="gameValue">{{ state }}</p>
  </div>
</template>

<style scoped>
  .gameSquare {
    border: 2px solid #b35d90;
    overflow: hidden;
    border-radius: 8px;
  }

  .gameSquare:hover {
    background-color: #f4b9cc;
  }

  .gameValue {
    font-size: 2rem;
    color: #b35d90; 
  }
</style>