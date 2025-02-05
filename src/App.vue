<script setup lang="ts">
import { ref } from "vue";
import Question from "./components/Question.vue";
import Progress from "./components/Progress.vue";
import Answer from "./components/Answer.vue";
import LevelUp from "./components/LevelUp.vue";
import Introduction from "./components/Introduction.vue";

let showAnswer = ref<boolean>(false);
let showLevelUp = ref<boolean>(false);
let showIntroduction = ref<boolean>(false);

let newEmitAnswer = ref<number>(0);
let newEmitLevelUp = ref<number>(0);

let answerAnswer = ref<string>("");
let questionAnswer = ref<string>("");
let correctAnswer = ref<boolean>(false);
let levelUpAnswer = ref<boolean>(false);

function showAnswerFunc(info: {
    answer: string;
    question: string;
    correct: boolean;
    levelUp: boolean;
}) {
    showAnswer.value = true;
    questionAnswer.value = info.question;
    answerAnswer.value = info.answer;
    correctAnswer.value = info.correct;
    newEmitAnswer.value++;
    levelUpAnswer.value = info.levelUp;
}
function showLevelUpFunc() {
    showLevelUp.value = true;
    newEmitLevelUp.value++;
}

// init
if (
    localStorage.getItem("learntLetters") === null ||
    localStorage.getItem("xp") === null ||
    localStorage.getItem("level") === null
) {
    localStorage.setItem("learntLetters", JSON.stringify(["E", "T"]));
    localStorage.setItem("xp", "0");
    localStorage.setItem("level", "1");
    showIntroduction.value = true;
}
</script>

<template>
    <div v-show="showIntroduction">
        <Introduction @hideIntroduction="showIntroduction = false" />
    </div>
    <div v-show="showLevelUp">
        <LevelUp
            :newEmitLevelUp="newEmitLevelUp"
            @hideLevelUp="showLevelUp = false" />
    </div>
    <div v-show="showAnswer">
        <Answer
            :question="questionAnswer"
            :answer="answerAnswer"
            :correct="correctAnswer"
            :newEmit="newEmitAnswer"
            :levelUp="levelUpAnswer"
            @hideAnswer="showAnswer = false"
            @showLevelUp="showLevelUpFunc()" />
    </div>
    <Question
        :showPopup="showAnswer || showLevelUp || showIntroduction"
        @showAnswer="(info) => showAnswerFunc(info)" />
    <Progress />
</template>

<style scoped></style>
