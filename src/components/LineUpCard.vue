<template>
    <div class="box">
      <div v-if="lineup.length === 0">
        <p>No lineups available.</p>
      </div>
      <div v-else>
        <div v-for="(lineupData, index) in lineup" :key="index">
          <div>
            <h2 class="subtitle mb-0">Lineup {{ index + 1 }}</h2>
            <h3 class="is-flex is-justify-content-center">GOALIE</h3>
            <div class="is-flex is-justify-content-center" v-if="lineupData.goalie">
              <div class="player-box box">
                <span>{{ lineupData.goalie && lineupData.goalie[0] ? lineupData.goalie[0].name : '' }}</span>
              </div>
            </div>
            <div v-if="lineupData.defense.length > 0">
              <h3 class="is-flex is-justify-content-center">DEFENSE</h3>
              <ul class="is-flex is-justify-content-center" >
                <li v-for="(player, index) in lineupData.defense" :key="index">
                  <div class="player-box box">
                    <span>{{ player.name }}</span>
                  </div>
                </li>
              </ul>
            </div>
            <div v-if="lineupData.midfield.length > 0">
            <h3 class="is-flex is-justify-content-center">MIDFIELD</h3>
              <ul class="is-flex is-justify-content-center" >
                <li v-for="(player, index) in lineupData.midfield" :key="index">
                  <div class="player-box box"> 
                    <span>{{ player.name }}</span>
                  </div>
                </li>
              </ul>
            </div>
            <div v-if="lineupData.forward.length > 0">
                <h3 class="is-flex is-justify-content-center">FORWARD</h3>
              <ul class="is-flex is-justify-content-center" >
                <li v-for="(player, index) in lineupData.forward" :key="index">
                  <div class="player-box box">
            
                    <span>{{ player.name }}</span>
                  </div>
                </li>
              </ul>
            </div>
          </div>
          <div>
            <button class="button delete" @click="removeLineup(index)"></button>
        </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps, defineEmits } from 'vue';
  
  const emit = defineEmits(['removeLineup']);
  
  const props = defineProps({
    lineup: Array,
    default: () => []
  });
  
  const removeLineup = (index) => {
    emit('removeLineup', index);
  };
  </script>
  
  <style scoped>
.player-box {
    width: 80px;
    height: 30px;
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
  background-color: rgb(240, 129, 129);
}

</style>
  