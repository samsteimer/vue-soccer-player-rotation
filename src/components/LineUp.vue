<template>
  <div class="box">
    <h2 class="subtitle mb-0">Lineup</h2>
    <div v-if="formation">
      <div v-for="(players, position) in formation" :key="position">
        <h3 class="is-flex is-justify-content-center">{{ position.toUpperCase() }}</h3>
        <div class="is-flex is-justify-content-center" v-if="position === 'goalie'">
          <div class="player-box box" @dragover.prevent="onDragOver" @drop="onDrop(position, index, $event)">
            <button class="delete is-small" v-if="players[0]" @click="addToTeam(players[0], position)"></button>
            <span v-if="players[0]">{{ players[0].name }}</span>
          </div>
        </div>
        <div v-else class="is-flex is-justify-content-center">
          <div v-for="(player, index) in players" :key="index" class="player-box box" @dragover.prevent="onDragOver" @drop="onDrop(position, index, $event)">
            <button class="delete is-small" v-if="player" @click="addToTeam(player, position)"></button>
            <span v-if="player">{{ player.name }}</span>
          </div>
        </div>
      </div>
    </div>
    <div>
      <button class="button" @click="saveLineup">Save Lineup</button>
    </div>
  </div>
</template>

  
<script setup>
import { defineEmits } from 'vue';
  
const emit = defineEmits(['playerDropped', 'addToTeam', 'saveLineup']);
  
const props = defineProps({
  formation: {
    type: Object,
    default: null
  }
});

const onDrop = (position, index, event) => {
  console.log('Event: ', event);
  event.preventDefault();    if (position === 'goalie') { // Check if position is 'goalie'
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

const saveLineup = () => {
  emit('saveLineup')
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
    margin-bottom: 1px;
    margin-right: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.9rem;
}

.delete {
    margin-right: 0.25rem;
}

.button {
  margin-top: 10px;
}


</style>
  