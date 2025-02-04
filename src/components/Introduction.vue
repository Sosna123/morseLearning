<script setup lang="ts"></script>

<template>
    <div class="introductionBg">
        <div class="introductionContainer">
            <div>
                <h1>Welcome!</h1>
                <h2>
                    You are answering question <br />
                    about the morse code alphabet
                </h2>
                <h2>
                    At the bottom are the letters <br />
                    that you're learning (the green ones) <br />
                    And your level along with the xp bar
                </h2>
                <h2>Your first 2 letter are:</h2>
                <h2>E .</h2>
                <h2>T -</h2>
                <v-btn @click="$emit('hideIntroduction')" theme="dark"
                    >back</v-btn
                >
            </div>
        </div>
    </div>
</template>

<style scoped>
.introductionBg {
    z-index: 6;
    width: 100%;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.75);
    position: absolute;
}
.introductionContainer {
    color: white;
    z-index: 7;
    background-color: #424242; /* grey-darken-3 */
    width: 70%;
    height: 60%;
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
