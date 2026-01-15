<script setup>
import {ref} from 'vue';

const props = defineProps({
    dice: {
        type: Object,
        default: () => ref([]),
    },
});

const emit = defineEmits(['changeDice']);

const roll_dice = () => {
    let array = props.dice;
    for (let index = 0; index < 5; index++) {
        if (array.length < 5 || !array[index].retain) {
            let rand = Math.floor(Math.random() * 6 + 1);
            array[index] = {index: index, number: rand, retain: false};
        }
    }
    emit('changeDice', array);
};
</script>

<template>
    <button type="button" @click="roll_dice">roll</button>
</template>

<style scoped></style>
