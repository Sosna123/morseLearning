<script setup lang="ts">
import { ref } from "vue";
import Question from "./components/Question.vue";
import Progress from "./components/Progress.vue";
import Answer from "./components/Answer.vue";

let showQuestion = ref<boolean>(true);

let answerAnswer = ref<string>("");
let questionAnswer = ref<string>("");
let correctAnswer = ref<boolean>(false);
let newEmitAnswer = ref<number>(0);

function showAnswerFunc(info: {
    answer: string;
    question: string;
    correct: boolean;
}) {
    showQuestion.value = false;
    questionAnswer.value = info.question;
    answerAnswer.value = info.answer;
    correctAnswer.value = info.correct;
    newEmitAnswer.value++;
}
</script>

<template>
    <div v-show="showQuestion">
        <Question @showAnswer="(info) => showAnswerFunc(info)" />
    </div>
    <div v-show="!showQuestion">
        <Answer
            :question="questionAnswer"
            :answer="answerAnswer"
            :correct="correctAnswer"
            :newEmit="newEmitAnswer"
            @hideAnswer="showQuestion = true" />
    </div>
    <Progress />
</template>

<style scoped></style>
