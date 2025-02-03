<script setup lang="ts">
import { morseObj } from "../objects.ts";
const lettersDisplay: string[] = [];
let learntLetters = JSON.parse(localStorage.getItem("learntLetters")!);

Object.entries(morseObj).forEach(([key, _value]) => {
    lettersDisplay.push(key);
});
</script>

<template>
    <div class="progressContainer">
        <div>
            <div class="progressBarDiv">
                <span>Your level: 1</span>
                <v-progress-linear
                    model-value="20"
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
