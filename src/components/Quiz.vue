<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
const stage = ref(1)
const currentQuestionIndex = ref(0)
const questionScoreCount = ref(0)
const quizList = ref([]);

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
onMounted(() => {
    const fetchQuizData = async () => {
      try {
        const response = await fetch('https://opentdb.com/api.php?amount=10&type=multiple');
        const data = await response.json();
         
        quizList.value = data.results.map((item) => ({
          question: item.question,
          options: [...item.incorrect_answers, item.correct_answer].sort(() => Math.random() - 0.5),
          correctAnswer: item.correct_answer,
          selectedAnswer: null, // Initialize to track user's selection
        }));
        // console.log(quizList.value);
      } catch (error) {
        console.error('Error fetching quiz data:', error);
      }
    };
    fetchQuizData()
})
</script>

<template>
    <div class="container">
        <header>
            <h2>Quiz App</h2><span>Question: {{ currentQuestionIndex + 1 }}</span>
        </header>

        <div v-if="quizList.length > 0">
         
            <div class="question-container" v-if="stage == 1">
                <p v-html="quizList[currentQuestionIndex].question"></p>
                <div v-for="(option, index) in quizList[currentQuestionIndex].options" :key="index">
                    <div
                        @click="chooseOption(option)"
                        :class="quizList[currentQuestionIndex].selectedAnswer == option ? 'selected' : 'list'"
                        v-html="option">
                    </div>
                </div>
            </div>
        
            <div v-if="stage == 2">
                <p>Score {{ questionScoreCount }}/{{ quizList.length }}</p>
                <div v-for="quiz in quizList">
                    <h4 v-html="quiz.question"></h4>
                    <p v-html="quiz.correctAnswer"></p>
                </div>
                <button class="btn" @click="goHome">Start Again</button>
            </div>

        
        <div v-if="stage == 1">
            <button class="btn" @click="prev" :disabled="currentQuestionIndex == 0">Prev</button>
            <button class="btn" @click="next" :disabled="currentQuestionIndex == quizList.length - 1">Next</button>
            <button v-if="currentQuestionIndex == quizList.length - 1" class="btn" @click="submit">Submit</button>
        </div>
        </div>
        <div v-else>
            <div class="spinner">
                <span class="loader"></span>
            </div>
            
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
/*
  Enter and leave animations can use different
  durations and timing functions.
*/
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
.loader {
  width: 48px;
  height: 48px;
  display: inline-block;
  position: relative;
}
.loader::after,
.loader::before {
  content: '';  
  box-sizing: border-box;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: 2px solid #0d1a26;
  position: absolute;
  left: 0;
  top: 0;
  animation: animloader 2s linear infinite;
}
.loader::after {
  animation-delay: 1s;
}

@keyframes animloader {
  0% {
    transform: scale(0);
    opacity: 1;
  }
  100% {
    transform: scale(1);
    opacity: 0;
  }
} 
.spinner{
    display: flex;
    justify-content: center;
    margin-top: 10px;
}
</style>