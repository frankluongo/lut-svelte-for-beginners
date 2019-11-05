<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
  import Container from "./Global/Container.svelte";
  import Question from "./Quiz/Question.svelte";

  let activeQuestion = 0;
  let score = 0;
  let quiz = getQuiz();

  onMount(() => {
    console.log("mounted");
  });
  beforeUpdate(() => {
    console.log("before updating");
  });
  afterUpdate(() => {
    console.log("after updated");
  });
  onDestroy(() => {
    console.log("DESTROY");
  });

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=12&type=multiple"
    );
    return await res.json();
  }

  function handleStartNewQuizClick() {
    quiz = getQuiz();
    resetQuiz();
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function resetQuiz() {
    quiz = getQuiz();
    score = 0;
    activeQuestion = 0;
  }

  function setScore() {
    score = score + 1;
  }

  // Reactive Statement
  $: if (score > 1) {
    alert("You win!");
    resetQuiz();
  }

  $: questionNumber = activeQuestion + 1;
</script>

<style>
  .fade-wrapper {
    position: absolute;
  }
</style>

<Container>
  <h2>Take The Quiz!</h2>
  <h3>Current Score: {score} | Question {questionNumber}</h3>
  <button on:click={handleStartNewQuizClick}>Start New Quiz</button>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div in:fly={{ y: 100 }} out:fly={{ y: -100 }} class="fade-wrapper">
          <Question
            details={question}
            {nextQuestion}
            {setScore}
            {activeQuestion} />
        </div>
      {/if}
    {/each}
  {/await}

</Container>
