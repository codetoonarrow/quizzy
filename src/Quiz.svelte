<script>
    import { fade, blur, fly, slide, scale } from "svelte/transition";
    import Question from "./Question.svelte";

    let activeQuestion = 0;
    let score = 0;
    let quiz = getQuiz();

    function pickAnswer(answer) {
        if (answer === correctAnswer){
            return result = "Correct!"
        }
        result = "Wrong!";
    }

    async function getQuiz(){
        const res = await fetch(
            "https://opentdb.com/api.php?amount=10&category=12&type=multiple"
            );
        const quiz = await res.json();
        return quiz;
    }


    function nextQuestion(){
        activeQuestion = activeQuestion + 1;
    }

    function resetQuiz(){
        score = 0;
        activeQuestion = 0;
        quiz =getQuiz();
    }

    function addToScore(){
        score = score + 1;
    }
    // "$" Marks any statement as reactive. A labeled statemet AKA
    $: if (score > 7){
        alert('You won!');
        resetQuiz();
    }
    $: questionNumber = activeQuestion + 1;
</script>

<div>
    <button on:click={resetQuiz}>Start New Quiz</button>
    <h3>My score: {score}</h3>
    <!-- Reactive Statment -->
    <h4>Question #{questionNumber}</h4>

    {#await quiz} 
        Loading ...
    {:then data}   

        {#each data.results as question, index}
            {#if index === activeQuestion}
                <div in:fly={{x: 100}} out:fly={{x: -200}} class="fade-wrapper">
                    <Question {addToScore} {nextQuestion} {question} />
                </div>
            {/if}
        {/each}
    {/await}
</div>


<style>
    .fade-wrapper{
        position:absolute;
    }
</style>