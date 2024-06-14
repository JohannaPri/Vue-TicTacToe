<script setup lang="ts">
  import { defineProps, defineEmits } from 'vue';

  const props = defineProps<{
    state: string;
  }>();

  const emit = defineEmits(["squareClicked"]);

  const playGame = () => {
    if (props.state === '') {
      emit("squareClicked");
    }
  };
</script>

<template>
  <div class="gameSquare" @click="playGame">
    <p class="gameValue">{{ state }}</p>
  </div>
</template>

<style scoped>
  .gameSquare {
    border: 2px solid #b35d90;
  }

  .gameSquare:hover {
    background-color: #f4b9cc;
  }

  .gameValue {
    font-size: 2rem;
    color: #b35d90; 
  }
</style>