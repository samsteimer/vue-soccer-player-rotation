<template>
    <div class="box formation-box">
      <h3 class="subtitle">Formation</h3>
      <form @submit.prevent="createFormation">
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label">Defense</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <input class="input" type="number" v-model="defense" placeholder="Defense count">
              </div>
            </div>
          </div>
        </div>
  
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label">Midfield</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <input class="input" type="number" v-model="midfield" placeholder="Midfield count">
              </div>
            </div>
          </div>
        </div>
  
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label class="label">Forward</label>
          </div>
          <div class="field-body">
            <div class="field">
              <div class="control">
                <input class="input" type="number" v-model="forward" placeholder="Forward count">
              </div>
            </div>
          </div>
        </div>
  
        <div class="field is-horizontal">
          <div class="field-body">
            <div class="field">
              <div class="control">
                <button class="button" type="submit">Create</button>
              </div>
            </div>
          </div>
        </div>
      </form>
    </div>
</template>
  
<style scoped>
.formation-box {
    padding: 0.75rem;
}
  
.field {
        margin-bottom: 0.5rem;
}

.field.is-horizontal > .field-body > .field {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}


</style>
  

<script setup>
import { ref } from 'vue';

const goalie = ref([null]);
const defense = ref ([]);
const midfield = ref ([]);
const forward = ref ([]);

const emit = defineEmits (['formationSubmitted'])

const createFormation = () => {
    const defenseCount = parseInt(defense.value);
    const midfieldCount = parseInt(midfield.value);
    const forwardCount = parseInt(forward.value);

    const formationData = {
        goalie: goalie.value,
        defense: Array(defenseCount).fill(null),
        midfield: Array(midfieldCount).fill(null),
        forward: Array(forwardCount).fill(null)
    };

    emit('formationSubmitted', formationData);
    
    goalie.value = [null];
    defense.value = '';
    midfield.value = '';
    forward.value = '';
}

</script>