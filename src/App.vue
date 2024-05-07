<template>
  <div>
    <TitleHeader />
    <EnterPlayers @playerSubmitted="handlePlayerSubmitted" />
    <EnterFormation @formationSubmitted="handleFormationSubmitted"/>
    <ListPlayers :team="team" @playerDeleted="handlePlayerDeleted" />
    <LineUp :formation="formation" @playerDropped="handlePlayerDropped"/>
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
    defense: Array(formationData.defense).fill(null),
    midfield: Array(formationData.midfield).fill(null),
    forward: Array(formationData.forward).fill(null)
  };

  saveTeamToLocalStorage();
};

const handlePlayerDropped = (player, position) => { // Change the parameter to accept the entire player object
  console.log('Player: ', player);
  if (player) {
    const positionIndex = formation.value[position].findIndex(slot => slot === null);
    if (positionIndex !== -1) {
      formation.value[position][positionIndex] = player; // Store the player object
      saveTeamToLocalStorage();
    } else {
      console.error(`No available space in ${position} for the player.`);
    }
  } else {
    console.error(`Player not found.`);
  }
}


</script>