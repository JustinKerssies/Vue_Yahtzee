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

const numberCheck = number => {
    return count.value[number] * number;
};
const straightCheck = (straightlength, value) => {
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
    return counter == straightlength ? value : 0;
};

const ofaKindCheck = (ofakindamount, value) => {
    for (let index = 1; index <= 6; index++) {
        if (count.value[index] >= ofakindamount) {
            return value;
        }
    }
    return 0;
};

const sumP1Values = computed(() => {
    let endvalue = 0;
    for (let index = 1; index <= 6; index++) {
        endvalue += numberCheck(index);
    }
    return endvalue;
});

const sumP2Values = computed(() => {
    let endvalue = 0;
    endvalue += fullHouseCheck();
    endvalue += straightCheck(4, 30);
    endvalue += straightCheck(5, 40);
    endvalue += ofaKindCheck(3, sumOfAllDice.value);
    endvalue += ofaKindCheck(4, sumOfAllDice.value);
    endvalue += ofaKindCheck(5, 50);
    return endvalue;
});

const sumOfAllDice = computed(() => {
    diceAmounts();
    return diceArray.value.reduce((acc, item) => acc + item, 0);
});

const fullHouseCheck = () => {
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
};
</script>
<template>
    <table>
        <tr>
            <th>P1 names</th>
            <th>points</th>
            <th>P2 names</th>
            <th>points</th>
        </tr>
        <tr>
            <td>Ones</td>
            <td>{{ numberCheck(1) }}</td>
            <td>FullHouse</td>
            <td>{{ fullHouseCheck() }}</td>
        </tr>
        <tr>
            <td>Twos</td>
            <td>{{ numberCheck(2) }}</td>
            <td>FourStraight</td>
            <td>{{ straightCheck(4, 30) }}</td>
        </tr>
        <tr>
            <td>Threes</td>
            <td>{{ numberCheck(3) }}</td>
            <td>FiveStraight</td>
            <td>{{ straightCheck(5, 40) }}</td>
        </tr>
        <tr>
            <td>Fours</td>
            <td>{{ numberCheck(4) }}</td>
            <td>ThreeofaKind</td>
            <td>{{ ofaKindCheck(3, sumOfAllDice) }}</td>
        </tr>
        <tr>
            <td>Fives</td>
            <td>{{ numberCheck(5) }}</td>
            <td>FourofaKind</td>
            <td>{{ ofaKindCheck(4, sumOfAllDice) }}</td>
        </tr>
        <tr>
            <td>Sixes</td>
            <td>{{ numberCheck(6) }}</td>
            <td>Yathzee</td>
            <td>{{ ofaKindCheck(5, 50) }}</td>
        </tr>
        <tr>
            <td>Total</td>
            <td>{{ sumP1Values }}</td>
            <td>Total</td>
            <td>{{ sumP2Values }}</td>
        </tr>
    </table>
</template>

<style scoped></style>
