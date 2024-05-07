<template>
    <div>

      <h2>Lineup</h2>
      <div v-if="formation">
        <div v-for="(players, position) in formation" :key="position">
          <h3>{{ position }}</h3>
          <div v-for="(player, index) in players" :key="index" class="player-box" @dragover.prevent="onDragOver" @drop="dropPlayer(position, $event)">
            {{ player ? player.name : '-' }} <!-- Display player's name or '-' if empty -->
          </div>
        </div>
      </div>
    </div>
  </template>
  
  
  <script setup>
  import { defineProps, defineEmits } from 'vue';
  
  const emit = defineEmits(['playerDropped']);
  
  const props = defineProps({
    formation: {
      type: Object,
      default: null
    }
  });
  const dropPlayer = (position) => (event) => {
  event.preventDefault();
  const playerData = event.dataTransfer.getData('application/json');
  console.log('Player data:', playerData);
  
  // Parse the player object
  const player = JSON.parse(playerData);

  console.log('Player dropped:', player);
  emit('playerDropped', player, position);
};


    const onDragOver = (event) => {
        console.log('Drag over event');
    }

  </script>
  
  <style scoped>
  .player-box {
    width: 100px;
    height: 50px;
    background-color: lightgray;
    margin-bottom: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  </style>
  