<script>

import ScoreBoard from './components/ScoreBoard.vue'

export default{
    name: 'app',
    components:{
        ScoreBoard
    },

    data(){
        return{
            question: String,
            incorrectAnswers:[],
            correctAnswer:[],
            chosenAnswer: undefined,
            answerSubmitted: false,
            winCount: 0,
            loseCount: 0
        }
    },

    computed:{
        answers() {
            //contournement de l'attributation de valeur bidirectionnel
            let answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
            // astuce pour générer un nombre aléatoire pour définir la position dans le tableau
            answers.splice( Math.round(Math.random()* answers.length), 0, this.correctAnswer);
            return answers;
        }
    },

    methods:{
        submitAnswer(){
            if(!this.chosenAnswer){
                alert('Pick on of the options');
            } else {
                this.answerSubmitted = true;
                if(this.chosenAnswer === this.correctAnswer) {
                    this.winCount++;
                    sessionStorage.setItem("winCount", JSON.stringify(this.winCount));
                } else {
                    this.loseCount++;
                    sessionStorage.setItem("loseCount",JSON.stringify(this.loseCount))
                }
            }
        },
        getNewQuestion(){
            this.answerSubmitted = false;
            this.chosenAnswer = undefined;
            this.question = undefined;
            this.axios.get('https://opentdb.com/api.php?amount=1&category=18')
            .then((response) => {
                this.question = response.data.results[0].question;
                this.incorrectAnswers= response.data.results[0].incorrect_answers;
                this.correctAnswer = response.data.results[0].correct_answer;
            }) 
        }
    },

    created(){
        this.winCount= sessionStorage.getItem("winCount") ? JSON.parse(sessionStorage.getItem("winCount")) : this.winCount;
        this.loseCount= sessionStorage.getItem("loseCount") ? JSON.parse (sessionStorage.getItem("loseCount")) : this.loseCount;
        this.getNewQuestion();
    }
}

</script>

<template>
    
<div>    

    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount"/>

    <h1 v-html="this.question"></h1>

    <template v-if="this.question">
        <template v-for="answer in this.answers" :key="answer">
            <input 
                :disabled="this.answerSubmitted"
                type="radio" 
                name="options" 
                :value="answer"
                v-model="chosenAnswer"
            >

            <label v-html="answer"> </label><br>
        </template>

        <button
            v-if="!this.answerSubmitted" 
            @click="this.submitAnswer()" 
            class="send" 
            type="button">Send
        </button>


        <section class="result" v-if="answerSubmitted">
            <h4 v-if="this.chosenAnswer === this.correctAnswer"
                v-html=" '&#9989; Congratulations, the answer ' + this.chosenAnswer + ' is correct.'"
            > 
            </h4>

            <h4 v-else
            v-html="`&#10060; I'm sorry, you picked the wrong answer. The correct is `+ this.correctAnswer "
            > 
            </h4>

            <button @click="this.getNewQuestion()" class="send" type="button">Next question</button>

        </section>

    </template>
</div>

</template>

<style>
#app {
    font-family: Arial, Helvetica, sans-serif;
    text-align: center;
    color: black;
    margin: 60px auto;
    max-width: 960px;
}

input[type=radio]{
    margin: 12px 4px;
}

button.send{
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: blue;
    border: 1px solid blue;
    cursor: pointer;
}

</style>
