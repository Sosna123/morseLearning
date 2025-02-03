<script setup lang="ts">
import { ref } from "vue";
import { morseObj } from "../objects.ts";

const emit = defineEmits(["showAnswer"]);

interface MorseObj {
    [key: string]: string;
}
const morseObjTyped: MorseObj = morseObj;

let inputValue = ref<string>("");
let currQuestion = ref<string>("");
let questionType = ref<null | "morse" | "alphabet">(null);

function makeQuestion() {
    // ? temporary array; to be replaced with letters and level
    let tempArr: string[] = ["E", "T", "M", "A", "I"];
    let letter = tempArr[Math.round(Math.random() * (tempArr.length - 1))];
    let question: string;
    if (Math.round(Math.random()) === 1) {
        questionType.value = "morse";
        question = `What\`s '${letter}' in morse code?`;
    } else {
        questionType.value = "alphabet";
        question = `What letter is '${morseObjTyped[letter]}'`;
    }
    currQuestion.value = question;
}

function checkAnswer(answer: string, question: string) {
    let questionImportant: string = "";
    let isInQuotations = false;
    let questionArr = question.split("");

    questionArr.forEach((e) => {
        if (e === "'") {
            isInQuotations = !isInQuotations;
        }
        if (isInQuotations && e != "'") {
            questionImportant += e;
        }
    });

    answer = answer.toUpperCase();
    questionImportant = questionImportant.toUpperCase();
    let emitObj = {
        answer: answer,
        question: questionImportant,
        correct: false,
    };

    if (questionType.value == "morse") {
        let helpMeSwapValues = answer;
        answer = questionImportant;
        questionImportant = helpMeSwapValues;
    }

    emitObj.correct = morseObjTyped[answer] === questionImportant;
    emit("showAnswer", emitObj);

    return morseObjTyped[answer] === questionImportant;
}

function typeInField(event: KeyboardEvent) {
    let charsArr: string[] = [
        "Enter",
        "Backspace",
        "Delete",
        "ArrowLeft",
        "ArrowRight",
    ];
    if (questionType.value == "morse") {
        if (!(event.key.match(/[.-]/) || charsArr.includes(event.key))) {
            let inputValueArr = inputValue.value.split("");
            inputValueArr.pop();
            inputValue.value = inputValueArr.join("");
            return;
        }
        if (event.key === "Enter") {
            submit();
        }
    } else {
        if (!(event.key.match(/[a-zA-Z0-9]/) || charsArr.includes(event.key))) {
            let inputValueArr = inputValue.value.split("");
            inputValueArr.pop();
            inputValue.value = inputValueArr.join("");
            return;
        }
        if (inputValue.value.length > 1) {
            inputValue.value = event.key;
        }
        if (event.key === "Enter") {
            submit();
        }
    }
}

function submit() {
    let isCorrect = checkAnswer(inputValue.value, currQuestion.value);
    console.log("Correct?", isCorrect);
    inputValue.value = "";
    makeQuestion();
}

makeQuestion();
</script>

<template>
    <div>
        <h1>Testing</h1>
        <h2>{{ currQuestion }}</h2>
        <v-text-field
            label="Type in your answer"
            v-model="inputValue"
            @keyup="typeInField($event)"></v-text-field>
        <v-btn @click="submit()">submit</v-btn>
    </div>
</template>

<style scoped></style>
