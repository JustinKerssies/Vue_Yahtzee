<script setup>
import {ref, computed} from 'vue';

const {diceArray} = defineProps({
    diceArray: {
        type: Object,
        default: () => ref([]),
    },
});

const count = ref({});

const diceAmounts = () => {
    count.value = {
        1: diceArray.value.filter(n => n === 1).length,
        2: diceArray.value.filter(n => n === 2).length,
        3: diceArray.value.filter(n => n === 3).length,
        4: diceArray.value.filter(n => n === 4).length,
        5: diceArray.value.filter(n => n === 5).length,
        6: diceArray.value.filter(n => n === 6).length,
    };

    return count.value;
};

const straightCheck = straightlength => {
    let flag = false;
    let counter = 0;
    for (let index = 1; index <= 6; index++) {
        if (flag == false) {
            flag = true;
        } else if (count.value[index] == 0 && flag) {
            counter = 0;
            flag = false;
        }
        if (count.value[index] > 0 && flag) {
            counter++;
        }
        if (counter == straightlength) {
            break;
        }
    }
    return counter == straightlength ? true : false;
};

const ofaKindCheck = ofakindamount => {
    for (let index = 1; index <= 6; index++) {
        if (count.value[index] >= ofakindamount) {
            return true;
        }
    }
    return false;
};

const fullHouseCheck = computed(() => {
    let flag2 = false;
    let flag3 = false;
    for (let index = 0; index <= 6; index++) {
        if (count.value[index] == 3) {
            flag3 = true;
        } else if (count.value[index] == 2) {
            flag2 = true;
        }
    }
    return flag2 && flag3 ? 25 : 0;
});

const sumOfAllDice = computed(() => {
    diceAmounts();
    return diceArray.value.reduce((acc, item) => acc + item, 0);
});

const straight4Check = computed(() => {
    if (straightCheck(4)) {
        return 30;
    }
    return 0;
});

const straight5Check = computed(() => {
    if (straightCheck(5)) {
        return 40;
    }
    return 0;
});

const ofaKind3Check = computed(() => {
    if (ofaKindCheck(3)) {
        return sumOfAllDice;
    }
    return 0;
});

const ofaKind4Check = computed(() => {
    if (ofaKindCheck(4)) {
        return sumOfAllDice;
    }
    return 0;
});

const yatzhee = computed(() => {
    if (ofaKindCheck(5)) {
        return 50;
    }
    return 0;
});
</script>
<template>
    <p>diceArray =>{{ diceArray }}</p>
    <p>sum =>{{ sumOfAllDice }}</p>
    <table>
        <tr>
            <th>name</th>
            <th>points</th>
        </tr>
        <tr>
            <td>FullHouse</td>
            <td>{{ fullHouseCheck }}</td>
        </tr>
        <tr>
            <td>FourStraight</td>
            <td>{{ straight4Check }}</td>
        </tr>
        <tr>
            <td>FiveStraight</td>
            <td>{{ straight5Check }}</td>
        </tr>
        <tr>
            <td>ThreeofaKind</td>
            <td>{{ ofaKind3Check }}</td>
        </tr>
        <tr>
            <td>FourofaKind</td>
            <td>{{ ofaKind4Check }}</td>
        </tr>
        <tr>
            <td>Yathzee</td>
            <td>{{ yatzhee }}</td>
        </tr>
    </table>
</template>

<style scoped></style>
