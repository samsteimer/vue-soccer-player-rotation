<template>
    <div class="box list">
        <h3 class="subtitle">Bench</h3>
        <ul class="columns is-multiline">
            <li class="column half" v-for="player in team" v-bind:key="player.id" >
                <div class="box player-box" draggable="true" @dragstart="dragStart(player, $event)">
                    <button class="delete is-small" @click="deletePlayer(player.id)"></button>
                    <span>{{ player.name }} </span> 
                    <span class="position">{{ player.preferredPosition }}</span>
                </div> 
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

<style scoped>
.player-box {
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 10px;
    max-height: 30px;
    max-width: 210px;
    margin: -.75rem 0;
    margin-bottom: 0.25rem;
    background-color: #f4f4f4;
}

.player-box span {
    margin-left: 0.5rem;
}

.delete {
    margin-left: 0.1rem;
}

.box {
    padding: .5rem; 
    
}

.list {
    min-width: 460px;
}

.position {
    font-size: 0.8rem;
    text-transform: uppercase;
    margin-left: auto;
}

</style>