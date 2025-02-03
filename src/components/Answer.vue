<script setup lang="ts">
import { watch } from "vue";
import { morseObj } from "../objects.ts";

const props = defineProps<{
    question: string;
    answer: string;
    correct: boolean;
    newEmit: number;
}>();

const emit = defineEmits(["hideAnswer"]);

function changeText() {}

function changeClass() {
    return props.correct ? "correctAnswer" : "wrongAnswer";
}

function checkCorrectAnswer() {
    if (props.correct) {
        return props.answer;
    }

    let returnValue = "";
    if (props.question.match(/[.-]/)) {
        Object.entries(morseObj).forEach(([key, value]) => {
            if (value === props.question) {
                returnValue = key;
                return;
            }
        });
    } else {
        Object.entries(morseObj).forEach(([key, value]) => {
            if (key === props.question) {
                returnValue = value;
            }
        });
    }
    return returnValue;
}

watch(
    () => props.newEmit,
    () => {
        checkCorrectAnswer();
    }
);
</script>

<template>
    <div>
        <h1>Question: {{ props.question }}</h1>
        <h2>Your answer: {{ props.answer }}</h2>
        <h2>Correct answer: {{ checkCorrectAnswer() }}</h2>
        <h3 :class="changeClass()">
            {{ props.correct ? "Correct" : "Incorrect" }}
        </h3>
        <v-btn @click="$emit('hideAnswer')">back</v-btn>
    </div>
</template>

<style scoped>
.correctAnswer {
    color: green;
}
.wrongAnswer {
    color: red;
}
</style>
