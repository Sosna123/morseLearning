<script setup lang="ts">
import { ref } from "vue";
import { morseObj } from "../objects.ts";
const lettersDisplay: string[] = [];
let learntLetters = JSON.parse(localStorage.getItem("learntLetters")!);

Object.entries(morseObj).forEach(([key, _value]) => {
    lettersDisplay.push(key);
});

let xp = ref<number>(0);
let level = ref<number>(1);

function updateBar() {
    xp.value = parseInt(localStorage.getItem("xp")!) ?? 0;
    level.value = parseInt(localStorage.getItem("level")!) ?? 1;
}

setInterval(() => updateBar(), 10);
</script>

<template>
    <div class="progressContainer">
        <div>
            <div class="progressBarDiv">
                <span>Your level: {{ level }}</span>
                <v-progress-linear
                    :model-value="xp"
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
    z-index: 10;
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
