<template>
  <div>
    <TitleHeader />
    <EnterPlayers @playerSubmitted="handlePlayerSubmitted" />
    <ListPlayers :team="team" @playerDeleted="handlePlayerDeleted" />
  </div>


</template>

<script setup>
import TitleHeader from './components/TitleHeader.vue';
import EnterPlayers from './components/EnterPlayers.vue';
import ListPlayers from './components/ListPlayers.vue';

import { ref, onMounted } from 'vue';

const team = ref ([]);

onMounted(() => {
  const savedTeam = JSON.parse(localStorage.getItem('team'));

  if (savedTeam) {
    team.value = savedTeam;
  }
})

const handlePlayerSubmitted = (player) => {
  team.value.push({
    id: generateUniqueId(),
    name: player.name,
    position: player.position
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
}

const saveTeamToLocalStorage = () => {
  localStorage.setItem('team', JSON.stringify(team.value));
}
</script>