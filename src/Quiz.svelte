<script>
import {fade, blur, fly, slide, scale} from 'svelte/transition' 
import {onMount, beforeUpdate, afterUpdate, onDestroy} from "svelte"  
import App from "./App.svelte";
import Question from "./Question.svelte";
import Modal from "./Modal.svelte"
import {score} from "./store.js"

    let quiz = getQuiz();
    let activeQuestion = 0;
   
    let isModalOpen = false

    onMount(()=> {
        console.log('imounted')
    })
    beforeUpdate(()=> {
        console.log('before update')
    })
    afterUpdate(()=> {
        console.log('after update')
    })

    async function getQuiz(){
        const res = await fetch("https://opentdb.com/api.php?amount=10&category=12&type=multiple")
        const quiz = await res.json();
        return quiz;
    }

    function nextQuestion(){
        activeQuestion = activeQuestion + 1;
    }

    function resetQuiz(){
        isModalOpen=false;
        score.set(0);
        activeQuestion =0;
        quiz = getQuiz();
    }


    //Reactive Decleration 
    $: if($score > 1){
        isModalOpen=true
        
    }

    //Reactive Decleration
    $: questionNumber = activeQuestion + 1

</script>

<style>
    .fade-wrapper{
        position: absolute;
    }

    .container{
        min-height: 500px;
    }
</style>



<div class="container">
    <button on:click|once={resetQuiz}>Start New Quiz</button>
    

    <h3>My Score: {$score}</h3>
    <h4>Question #{questionNumber}</h4>
    {#await quiz}
        Loading...
    {:then data}

    {#each data.results as question, index}
        {#if index===activeQuestion}
            <div in:fly={{x: 100 }} out:fly={{x: -200}} class='fade-wrapper'>
             <Question {nextQuestion}{question}/>
         </div>
        {/if}
    {/each}
    
    {/await}
 
</div>


{#if isModalOpen}
<Modal on:close={resetQuiz}>
    <h2>You Won!</h2>
    <p>Congrats</p>
    <button on:click={resetQuiz}>Start Over</button>
</Modal>
{/if}
