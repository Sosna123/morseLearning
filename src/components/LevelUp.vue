<script setup lang="ts">
import { watch, ref } from "vue";
import { morseObj, unlockLetters } from "../objects.ts";

const props = defineProps<{
    newEmitLevelUp: number;
}>();

interface MorseObj {
    [key: string]: string;
}
const morseObjTyped: MorseObj = morseObj;

let level = ref<string>(localStorage.getItem("level")!);
let newLetter = ref<string>(unlockLetters[parseInt(level.value) - 1]);
let newLetterMorse = ref<string>(morseObjTyped[newLetter.value]);

watch(
    () => props.newEmitLevelUp,
    () => {
        level.value = localStorage.getItem("level")!;
        newLetter.value = unlockLetters[parseInt(level.value) - 1];
        newLetterMorse.value = morseObjTyped[newLetter.value];
    }
);
</script>

<template>
    <div class="levelUpBg">
        <div class="levelUpContainer">
            <div>
                <h1>
                    Congratulations on reaching level
                    {{ level }}
                </h1>
                <h2>
                    Your new learnt letter: {{ newLetter }} {{ newLetterMorse }}
                </h2>
                <v-btn @click="$emit('hideLevelUp')" theme="dark">back</v-btn>
            </div>
        </div>
    </div>
</template>

<style scoped>
.levelUpBg {
    z-index: 2;
    width: 100%;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.75);
    position: absolute;
}
.levelUpContainer {
    color: white;
    z-index: 3;
    background-color: #424242; /* grey-darken-3 */
    width: 60%;
    height: 40%;
    border-radius: 20px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 18px;
}
</style>
