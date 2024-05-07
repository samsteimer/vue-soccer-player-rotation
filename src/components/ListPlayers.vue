<template>
    <div>
        <h3>Team</h3>
        <ul>
            <li v-for="player in team" v-bind:key="player.id" draggable="true" @dragstart="dragStart(player, $event)">
            <span>{{ player.name }} </span> <span>{{ player.preferredPosition }}</span>
            <button @click="deletePlayer(player.id)">delete</button>
            </li>
        </ul>
    </div>

</template>

<script setup>

const emit = defineEmits(['playerDeleted', 'playerDragged'])

const props = defineProps({
    team: {
        type: Array,
        required: true
    }
});


const deletePlayer = (id) => {
    emit('playerDeleted', id);
};

const dragStart = (player, event) => {
    event.dataTransfer.setData('application/json', JSON.stringify(player)); // Set the entire player object

    console.log('dragstart: ', player);

    emit('playerDragged', player); // Emit the player object
}


</script>