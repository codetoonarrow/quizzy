<script>

    import Question from "./Question.svelte";
    
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

    function handleClick(){
        quiz = getQuiz();
    }

</script>

<div>
    {#await quiz} 
        Loading ...
    {:then data}   

        {#each data.results as question}
            <Question {question} />
        {/each}

    {/await}

    <button on:click={handleClick}>Get Questions</button>

</div>