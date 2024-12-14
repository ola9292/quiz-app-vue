<script setup>
import { ref } from 'vue';
const stage = ref(1)
const currentQuestionIndex = ref(0)
const questionScoreCount = ref(0)
const quizList = ref([
    {
        question: "What is the capital of France?",
        options: ["Berlin", "Madrid", "Paris", "Rome"],
        correctAnswer: "Paris",
        selectedAnswer: null
    },
    {
        question: "Which planet is known as the Red Planet?",
        options: ["Earth", "Mars", "Jupiter", "Venus"],
        correctAnswer: "Mars",
        selectedAnswer: null
    },
    {
        question: "What is the largest ocean on Earth?",
        options: ["Atlantic Ocean", "Indian Ocean", "Arctic Ocean", "Pacific Ocean"],
        correctAnswer: "Pacific Ocean",
        selectedAnswer: null
    },
    {
        question: "Who wrote 'Romeo and Juliet'?",
        options: ["William Shakespeare", "Charles Dickens", "Jane Austen", "Mark Twain"],
        correctAnswer: "William Shakespeare",
        selectedAnswer: null
    }
]);

const chooseOption = (option) => {
    quizList.value[currentQuestionIndex.value].selectedAnswer = option
}
const next = () => {
    currentQuestionIndex.value++
}
const prev = () => {
    currentQuestionIndex.value--
}
const goHome = () => {
    window.location.href = '/';
}
const submit = () => {
    questionScoreCount.value = 0;
    
    quizList.value.forEach((quiz) => {
        if(quiz.selectedAnswer == quiz.correctAnswer){
            questionScoreCount.value++
        }
    })
    //console.log(questionScoreCount.value)
    stage.value = 2
}
</script>

<template>
    <div class="container">
        <header>
            <h2>Quiz App</h2>
        </header>
        
        <div v-if="stage == 1">
            <p>{{ quizList[currentQuestionIndex].question }}</p>
            <div v-for="(option, index) in quizList[currentQuestionIndex].options">
                <div :key="index" @click="chooseOption(option)" :class="quizList[currentQuestionIndex].selectedAnswer == option ? 'selected' : 'list'">{{ option }}</div>
            </div>
        </div>
        <div v-if="stage == 2">
            <p>Score {{ questionScoreCount }}/{{ quizList.length }}</p>
            <button class="btn" @click="goHome">Start Again</button>
        </div>
        <div v-if="stage == 1">
            <button class="btn" @click="prev" :disabled="currentQuestionIndex == 0">Prev</button>
            <button class="btn" @click="next" :disabled="currentQuestionIndex == quizList.length - 1">Next</button>
            <button v-if="currentQuestionIndex == quizList.length - 1" class="btn" @click="submit">Submit</button>
        </div>
    
    </div>
    
</template>

<style>
.container{
    font-family: sans-serif;
    width: 500px;
    max-width: 90%;
    margin: 100px auto;
    border: 1px solid #e6e6e6;
    padding: 20px;
    border-radius: 8px;
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}
header{
    border-bottom: 1px solid #e6e6e6;
}
.selected{
    background-color:#336699;
    color: #fff;
    border-radius: 5px;
    margin-bottom: 10px;
    padding: 8px 8px;
}
.list{
   border:1px solid #e6e6e6; 
    border-radius: 5px;
    margin-bottom: 10px;
    padding: 8px 8px;
}
.btn{
    padding: 8px 12px;
    border: none;
    background-color: #0d1a26;
    color: #fff;
    margin-right: 10px;
}
.btn:disabled {
    background-color: #cccccc;
    color: #666666;
    cursor: not-allowed;
    border: 1px solid #aaaaaa;
}
</style>