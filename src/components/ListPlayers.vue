<template>
    <div>
        <h3>Team</h3>
        <ul>
            <li v-for="player in team" v-bind:key="player.id">
            <span>{{ player.name }} </span> <span>{{ player.position }}</span>
            <button @click="deletePlayer(player.id)">delete</button>
            </li>
        </ul>
    </div>

</template>

<script setup>
import { defineProps } from 'vue';

const emit = defineEmits(['playerDeleted'])

const props = defineProps({
    team: {
        type: Array,
        required: true
    }
});

const deletePlayer = (id) => {
    emit('playerDeleted', id);
}

</script>