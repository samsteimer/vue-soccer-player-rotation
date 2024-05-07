<template>
    <div>
        <div>
            <form @submit.prevent="onSubmit">
                <div>
                    <label for="name" >Name: </label>
                    <input type="text" id="name" v-model="name">
                    <label for="preferred-postion">Preferred Position</label>
                    <select name="preferred-position" id="preferred-position" v-model="preferredPosition">
                        <option value="goalie">Goalie</option>
                        <option value="defense">Defense</option>
                        <option value="midfield">Midfield</option>
                        <option value="forward">Forward</option>
                    </select>
                </div>
                <div>
                    <button type="submit">Add Player</button>
                </div>
            </form>
        </div>
    </div>

</template>

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