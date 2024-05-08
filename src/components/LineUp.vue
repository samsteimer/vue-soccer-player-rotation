<template>
  <div>
    <h2>Lineup</h2>
    <div v-if="formation">
      <div v-for="(players, position) in formation" :key="position">
        <h3>{{ position }}</h3>
        <div v-if="position === 'goalie'">
          <div class="player-box" @dragover.prevent="onDragOver" @drop="onDrop(position, index, $event)">
            <span v-if="players[0]">{{ players[0].name }}</span>
            <button v-if="players[0]" @click="addToTeam(players[0], position)">X</button>
            <button v-if="!players[0]" disabled>X</button>
          </div>
        </div>
        <div v-else>
          <div v-for="(player, index) in players" :key="index" class="player-box" @dragover.prevent="onDragOver" @drop="onDrop(position, index, $event)">
            <span v-if="player">{{ player.name }}</span>
            <button v-if="player" @click="addToTeam(player, position)">X</button>
            <button v-if="!player" disabled>X</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

  
  <script setup>
  import { defineEmits } from 'vue';
  
  const emit = defineEmits(['playerDropped', 'addToTeam']);
  
  const props = defineProps({
    formation: {
      type: Object,
      default: null
    }
  });



  const onDrop = (position, index, event) => {
    console.log('Event: ', event);
    event.preventDefault();
    if (position === 'goalie') { // Check if position is 'goalie'
      dropPlayer('goalie', index, event); // Call dropPlayer with position 'goalie'
    } else {
      dropPlayer(position, index, event);
    }
  };


  const dropPlayer = (position, index, event) => {
    const playerData = event.dataTransfer.getData('application/json');
    console.log('Player data:', playerData);
    
    // Parse the player object
    const player = JSON.parse(playerData);

    console.log('Player dropped:', player);
    emit('playerDropped', player, position, index);
  };

  const addToTeam = (player, position) => {
    emit('addToTeam', player, position);
  }


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
  