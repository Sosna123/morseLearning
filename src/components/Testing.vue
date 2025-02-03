<script setup lang="ts">
import { ref } from "vue";

let inputValue = ref<string>("");

let charsArr: string[] = [
    "-",
    ".",
    "Enter",
    "Backspace",
    "Delete",
    "ArrowLeft",
    "ArrowRight",
];

function typeInField(event: KeyboardEvent) {
    if (!charsArr.includes(event.key)) {
        let inputValueArr = inputValue.value.split("");
        inputValueArr.pop();
        inputValue.value = inputValueArr.join("");
        return;
    }
    if (event.key === "Enter") {
        console.log("Enter pressed");
    }
}

function changeInputValue(mode: "add" | "remove", char: string = "") {
    if (mode === "add") {
        inputValue.value += char;
    } else {
        let inputValueArr = inputValue.value.split("");
        inputValueArr.pop();
        inputValue.value = inputValueArr.join("");
    }
}
</script>

<template>
    <div>
        <h1>Testing</h1>
        <h2>A</h2>
        <v-text-field
            label="Type in your answer"
            v-model="inputValue"
            @keyup="typeInField($event)"></v-text-field>
        <v-btn @click="changeInputValue('add', '-')">-</v-btn>
        <v-btn @click="changeInputValue('add', '.')">.</v-btn>
        <v-btn @click="changeInputValue('remove')">remove</v-btn>
        <v-btn @click="console.log('submit')">submit</v-btn>
    </div>
</template>

<style scoped></style>
