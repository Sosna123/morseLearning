<script setup lang="ts">
import { ref } from "vue";
import { morseObj, levelProgress } from "../objects.ts";
const lettersDisplay: string[] = [];
let learntLetters = JSON.parse(localStorage.getItem("learntLetters")!);

Object.entries(morseObj).forEach(([key, _value]) => {
    lettersDisplay.push(key);
});

let xp = ref<number>(0);
let maxXp = ref<number>(50);
let barPrecentage = ref<number>(0);
let level = ref<number>(1);

function updateBar() {
    xp.value = parseInt(localStorage.getItem("xp")!) ?? 0;
    level.value = parseInt(localStorage.getItem("level")!) ?? 1;
    maxXp.value =
        levelProgress[
            Math.min(parseInt(localStorage.getItem("level")!) - 1, 8)
        ];
    barPrecentage.value = Math.round((xp.value / maxXp.value) * 100);
}

setInterval(() => updateBar(), 100);
</script>

<template>
    <div class="progressContainer">
        <div>
            <div class="progressBarDiv">
                <span>Your level: {{ level }}</span>
                <v-progress-linear
                    :model-value="barPrecentage"
                    :height="16"></v-progress-linear>
            </div>
            <div class="progressLetterSpans">
                <span
                    v-for="letter in lettersDisplay"
                    :class="
                        learntLetters.includes(letter)
                            ? 'letterLearnt letterSpan'
                            : 'letterNotLearnt letterSpan'
                    ">
                    {{ letter }}
                </span>
            </div>
        </div>
    </div>
</template>

<style scoped>
.progressContainer {
    z-index: 1;
    width: 100%;
    text-align: center;
    position: absolute;
    bottom: 0;
    margin: auto;
    display: flex;
    justify-content: center;
    align-items: center;
}

.letterSpan {
    margin: 0 5px 0 5px;
    font-size: 32px;
}

.letterLearnt {
    color: green;
}

.letterNotLearnt {
    color: gray;
}
</style>
