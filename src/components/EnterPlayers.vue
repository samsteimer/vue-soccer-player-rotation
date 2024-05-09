<template>
    <div class="box player-entry-box">
      <h3 class="subtitle">Enter Player</h3>
      <form @submit.prevent="onSubmit">
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label">Name</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <input class="input" type="text" id="name" v-model="name" placeholder="Enter player name">
              </div>
            </div>
          </div>
        </div>
  
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label">Position</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <div class="select">
                  <select v-model="preferredPosition">
                    <option value="goalie">Goalie</option>
                    <option value="defense">Defense</option>
                    <option value="midfield">Midfield</option>
                    <option value="forward">Forward</option>
                  </select>
                </div>
              </div>
            </div>
          </div>
        </div>
  
        <div class="field is-horizontal">
          <div class="field-body">
            <div class="control">
              <button class="button is-primary is-small" type="submit">Add Player</button>
            </div>
          </div>
        </div>
      </form>
    </div>
  </template>
  
  <style scoped>
  .player-entry-box {
    padding: 0.75rem;
  }
  
  .field {
    margin-bottom: 0.5rem;
  }
  
  .input, .select {
    width: 100%; /* Make input and select full width */
  }
  
  .button {
    border: solid 1px;
    border-radius: 10px;
    background-color: aquamarine;
    max-height: 35px;
  }
  </style>
  

<script setup>
import { ref } from 'vue';

const name = ref ('');
const preferredPosition = ref ('');

const emit = defineEmits(['playerSubmitted'])

const onSubmit = () => {
    if(!name.value) {
        alert('Must enter name.');
        return;
    }

    const playerData = {
        name: name.value,
        preferredPosition: preferredPosition.value
    }

    emit('playerSubmitted', playerData);

    name.value = '';
    preferredPosition.value = '';
}
</script>