<template>
    <div class="jumbotron">
        <h1>{{ currentQuestion.category }}</h1>
        <p v-html="currentQuestion.question "></p>
        <ul class="list-group">
            <li 
                class="list-group-item li"
                v-for="(answer, index) in shuffledAnswers" 
                :key="index"
                :class="[selectedIndex === index ? 'selected' : '']"
                @click.prevent="selectAnswer(index)"
                v-html="answer"
            >
            </li>
        </ul>
        <div class="mt-3">
            <button 
                class="btn btn-primary mr-3"
                @click="submitAnswer"
            >
                Answer
            </button>
            <button class="btn btn-success" @click="next">Next Question </button>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment:Function
    },
    data(){
        return{
            selectedIndex: null,
            shuffledAnswers: []
        }
    },
    methods: {
        selectAnswer(index){
            return this.selectedIndex = index
        },
        submitAnswer(){
            let isCorrect = false

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }

            this.increment(isCorrect)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler(){
                this.selectedIndex = null,
                this.shuffleAnswers()
            }
        }
    },
    computed: {
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    }
}
</script>

<style scoped>
     .li:hover{
         cursor: pointer;
         background: #ccc;
         font-weight: 700;
     }
     .selected{
         background: rgb(162, 162, 229);
     }
     .correct{
         background: rgb(66, 122, 66);
     }
     .incorrect{
         background: rgb(216, 58, 58);
     }
</style>