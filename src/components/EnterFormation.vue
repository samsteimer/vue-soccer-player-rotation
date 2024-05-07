<template>
    <div>
        <h3>Formation: </h3>
        <form @submit.prevent="createFormation">
            <div>
                <label for="defense-setup">Defense</label>
                <input type="number" id="defense-setup" v-model="defense">
                <label for="midfield-setup">Midfield</label>
                <input type="number" id="midfield-setup" v-model="midfield">
                <label for="forward-setup">Forward</label>
                <input type="number" id="forward-setup" v-model="forward">
            </div>
            <div>
                <button type="submit">Create</button>
            </div>
        </form>
    </div>

</template>

<script setup>
import { ref } from 'vue';

const defense = ref ([]);
const midfield = ref ([]);
const forward = ref ([]);

const emit = defineEmits (['formationSubmitted'])

const createFormation = () => {
    const defenseCount = parseInt(defense.value);
    const midfieldCount = parseInt(midfield.value);
    const forwardCount = parseInt(forward.value);

    const formationData = {
        defense: Array(defenseCount).fill(null),
        midfield: Array(midfieldCount).fill(null),
        forward: Array(forwardCount).fill(null)
    };

    emit('formationSubmitted', formationData);
    
    defense.value = '';
    midfield.value = '';
    forward.value = '';
}

</script>