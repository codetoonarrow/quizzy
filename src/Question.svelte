<script>
    export let question;
    let answers = question.incorrect_answers;
</script>


<h5>{@html question.question}</h5>

{#each answers as answer}
    <button>{@html answer}</button>
{/each}
