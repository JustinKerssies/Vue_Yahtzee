<script setup>
import {ref, computed} from 'vue';
import dice from './components/DiceComponent.vue';
import pointvalue from './components/PointValueComponent.vue';

const currentDice = ref([]);
const count = ref(0);

const diceHandler = newArray => {
    currentDice.value = newArray;
    count.value++;
};

const currentArray = computed(() => {
    let test = ref([]);
    for (let index = 0; index < currentDice.value.length; index++) {
        test.value.push(currentDice.value[index].number);
    }
    return test;
});
</script>

<template>
    <div>
        <dice :dice="currentDice" @changeDice="diceHandler" />
        <ul>
            <li v-for="dice in currentDice" :key="dice.index">
                <input type="checkbox" v-model="dice.retain" />
                <label>{{ dice.number }}</label>
            </li>
        </ul>
        <pointvalue :diceArray="currentArray" />
    </div>
</template>

<style scoped></style>
