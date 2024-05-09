<template>
  <div class="container center">
    <TitleHeader />
    <div class="container columns">
      <div class="first-column column">
        <EnterPlayers @playerSubmitted="handlePlayerSubmitted" />
        <EnterFormation @formationSubmitted="handleFormationSubmitted"/>
      </div>
      <div class="second-column column">
        <ListPlayers :team="team" @playerDeleted="handlePlayerDeleted" />
        <LineUp :formation="formation" @playerDropped="handlePlayerDropped" @addToTeam="handleAddToTeam"/>
      </div>
    </div>
  </div>


</template>

<script setup>
import TitleHeader from './components/TitleHeader.vue';
import EnterPlayers from './components/EnterPlayers.vue';
import ListPlayers from './components/ListPlayers.vue';
import EnterFormation from './components/EnterFormation.vue';
import LineUp from './components/LineUp.vue';

import { ref, onMounted } from 'vue';

const team = ref ([]);

const formation = ref({
  goalie: [],
  defense: [],
  midfield: [],
  forward: []
});

onMounted(() => {
  const savedTeam = JSON.parse(localStorage.getItem('team'));
  const savedFormation = JSON.parse(localStorage.getItem('formation'));

  if (savedTeam) {
    team.value = savedTeam;
  }
  if (savedFormation) {
    formation.value = savedFormation;
  }
})

const handlePlayerSubmitted = (player) => {
  team.value.push({
    id: generateUniqueId(),
    name: player.name,
    preferredPosition: player.preferredPosition
  });

  saveTeamToLocalStorage();

  alert('Player added.');
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

const handlePlayerDeleted = (id) => {
  team.value = team.value.filter((player) =>
  player.id !== id);

  saveTeamToLocalStorage();
};

const saveTeamToLocalStorage = () => {
  localStorage.setItem('team', JSON.stringify(team.value));
  localStorage.setItem('formation', JSON.stringify(formation.value));
};

const handleFormationSubmitted = (formationData) => {
  formation.value = {
    goalie: [],
    defense: formationData.defense.map(() => null),
    midfield: formationData.midfield.map(() => null),
    forward: formationData.forward.map(() => null),
  };

  saveTeamToLocalStorage();
};

const handlePlayerDropped = (player, position, index) => {
  console.log('Player: ', player);
  if (player) {
    let teamIndex = -1;

    // Find the index of the player in the team array
    const playerIndex = team.value.findIndex(p => p.id === player.id);
    if (playerIndex !== -1) {
      // Remove the player from the team array
      teamIndex = playerIndex;
      team.value.splice(playerIndex, 1);
    } else {
      console.error(`Player not found in team.`);
    }
    
    // Assign the player to the specified index in the formation
    if (position === 'goalie') {
      if (formation.value.goalie.length === 0) {
        formation.value.goalie.push(player);
      } else {
        console.error('Goalie position already occupied.');
      }
    } else {
      if (formation.value[position][index] === null) {
        formation.value[position][index] = player;
      } else {
        console.error(`Slot ${index} in ${position} is already occupied.`);
      }
    }

    saveTeamToLocalStorage();

    // If the player was successfully added to the lineup, update the team
    if (teamIndex !== -1) {
      console.log(`Player ${player.name} successfully added to ${position} at index ${index}.`);
    } else {
      console.error(`Player ${player.name} was not added to ${position} at index ${index}.`);
    }
  } else {
    console.error(`Player not found.`);
  }
};

const handleAddToTeam = (player, position) => {
  // Remove the specific player from the lineup at the given position
  const positionArray = formation.value[position];
  const playerIndex = positionArray.findIndex(slot => slot && slot.id === player.id);
  
  if (position === 'goalie') {
    formation.value.goalie = [];
    saveTeamToLocalStorage();
  } else {

    if (playerIndex !== -1) {
      // Instead of removing the player, set the slot to null
      positionArray[playerIndex] = null;
      saveTeamToLocalStorage();
    } else {
      console.error(`Player not found in ${position}.`);
      return; // Exit the function if the player is not found in the position
    }
  }
  // Add the player to the team if not already present
  const isPlayerInTeam = team.value.some(p => p.id === player.id);
  if (!isPlayerInTeam) {
    team.value.push(player);
    saveTeamToLocalStorage();
  }
};

</script>

<style scoped>
.first-column {
  max-width: 300px;
  margin-left: 20px;
}

.second-column {
  max-width: 500px;
  margin-right: 20px;
  margin-left: 20px
}
</style>