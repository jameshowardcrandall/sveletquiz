<script>
    import {score} from './store.js'
   export let question;
   export let nextQuestion;
   

   let isCorrect;
   let isAnswered = false;
   
   let answers = question.incorrect_answers.map(answer => {
       return {
           answer,
           correct: false
       }
   });

   let allAnswers = [
       ...answers,
       {
           answer:question.correct_answer,
           correct: true
       }
   ]

function shuffle(array){
    array.sort(()=> Math.random()- 0.5)
}

shuffle(allAnswers);

function checkQuestion(correct){
    isAnswered = true;
    isCorrect = correct;
    if(correct){
        score.update(val => val+1);
    }
}




</script>

<style>
.answer{
    display: block;
    text-align: center;
    padding: 15px 30px;

}
</style>

<h3>{@html question.question}</h3>

{#if isAnswered}
<h5>
    {#if isCorrect}
    You got it right
    {:else}
    You goofed up
    {/if}
</h5>
{/if}

{#each allAnswers as answer }
    <button class="answer" on:click={()=>checkQuestion(answer.correct)}>{@html answer.answer}</button>
    
{/each}

{#if isAnswered}
<div>
<button on:click={nextQuestion}>Next Question</button>
</div>
{/if}