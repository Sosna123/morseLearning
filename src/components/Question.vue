<script setup lang="ts">
import { ref } from "vue";
import { morseObj, unlockLetters, levelProgress } from "../objects.ts";

const emit = defineEmits(["showAnswer", "showLevelUp", "closeAllPopups"]);

const props = defineProps<{
    showPopup: boolean;
}>();

interface MorseObj {
    [key: string]: string;
}
const morseObjTyped: MorseObj = morseObj;

let inputValue = ref<string>("");
let currQuestion = ref<string>("");
let questionType = ref<null | "morse" | "alphabet">(null);
let xp = parseInt(localStorage.getItem("xp")!);

function makeQuestion() {
    let learntLetters: string[] = JSON.parse(
        localStorage.getItem("learntLetters")!
    );
    let letter =
        learntLetters[Math.round(Math.random() * (learntLetters.length - 1))];
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
        levelUp: false,
    };

    if (questionType.value == "morse") {
        let helpMeSwapValues = answer;
        answer = questionImportant;
        questionImportant = helpMeSwapValues;
    }

    emitObj.correct = morseObjTyped[answer] === questionImportant;

    morseObjTyped[answer] === questionImportant
        ? (xp += 2)
        : xp > 0
        ? (xp -= 1)
        : xp;
    localStorage.setItem("xp", xp.toString());

    if (
        xp >=
        levelProgress[Math.min(parseInt(localStorage.getItem("level")!) - 1, 8)]
    ) {
        emitObj.levelUp = true;

        let level: number = parseInt(localStorage.getItem("level")!);
        level++;
        localStorage.setItem("level", level.toString());

        xp =
            xp -
            levelProgress[
                Math.min(parseInt(localStorage.getItem("level")!) - 2, 8)
            ];
        localStorage.setItem("xp", xp.toString());

        let learntLetters: string[] = JSON.parse(
            localStorage.getItem("learntLetters")!
        );
        learntLetters.push(unlockLetters[level - 1]);
        localStorage.setItem("learntLetters", JSON.stringify(learntLetters));
    }

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

    if (event.key === "Enter") {
        if (!props.showPopup) {
            submit();
            return;
        } else {
            emit("closeAllPopups");
            return;
        }
    }

    if (questionType.value == "morse") {
        if (!(event.key.match(/[.-]/) || charsArr.includes(event.key))) {
            let inputValueArr = inputValue.value.split("");
            inputValueArr.pop();
            inputValue.value = inputValueArr.join("");
            return;
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
    <div class="questionBg">
        <div class="questionContainer">
            <h2>{{ currQuestion }}</h2>
            <v-text-field
                label="Type in your answer"
                v-model="inputValue"
                @keyup="typeInField($event)"
                theme="dark"
                variant="underlined"></v-text-field>
            <v-btn @click="submit()" theme="dark">submit</v-btn>
        </div>
    </div>
</template>

<style scoped>
.questionBg {
    color: white;
    width: 100%;
    height: 100vh;
    z-index: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #424242; /* grey-darken-3 */
}

.questionContainer {
    z-index: 1;
    text-align: center;
    width: 50%;
}

h2 {
    margin-top: 0;
    font-weight: normal;
}
</style>
