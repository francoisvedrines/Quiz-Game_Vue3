<script>

export default{
    name: 'app',

    data(){
        return{
            question: String,
            incorrectAnswers:[],
            correctAnswer:[],
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

    created(){
        this.axios.get('https://opentdb.com/api.php?amount=1&category=18')
        .then((response) => {
        this.question = response.data.results[0].question;
        this.incorrectAnswers= response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
        })
    }
}


</script>

<template>
    
<div>    
    <h1 v-html="this.question"></h1>
    <template v-for="answer in this.answers" :key="answer">
        <input 
            type="radio" 
            name="options" 
            value="answer">

        <label v-html="answer"> </label><br>
    </template>

    <button class="send" type="button">Send</button>
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
