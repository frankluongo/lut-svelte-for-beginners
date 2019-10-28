<script>
  import Container from "./Global/Container.svelte";

  let result = "";
  let correctAnswer = "b";
  let answers = ["a", "b", "c", "d"];

  function handleButtonClick(input) {
    pickAnswer(input);
  }

  function pickAnswer(value) {
    result = value === correctAnswer ? "Correct" : "WRONG!";
  }
</script>

<style>
  /* h4 {
    color: red;
  } */
</style>

<Container>
  {#if result}
    <h4>{result}</h4>
  {:else}
    <h4>Pick an answer</h4>
  {/if}
  {#each answers as answer}
    <button on:click={handleButtonClick.bind(this, answer)}>
      Answer {answer.toUpperCase()}
    </button>
  {/each}
</Container>
