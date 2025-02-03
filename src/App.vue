<script setup lang="ts">
import { ref } from "vue";
import Question from "./components/Question.vue";
import Progress from "./components/Progress.vue";
import Answer from "./components/Answer.vue";

let showQuestion = ref<boolean>(true);
</script>

<template>
    <div v-show="showQuestion">
        <Question />
    </div>
    <div v-show="!showQuestion">
        <Answer />
    </div>
    <Progress />
</template>

<style scoped></style>
