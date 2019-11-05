<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import Container from "./Global/Container.svelte";
  import Question from "./Quiz/Question.svelte";
  import Modal from "./Global/Modal.svelte";

  let activeQuestion = 0;
  let score = 0;
  let quiz = getQuiz();
  let isModalOpen = false;

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
    score = 0;
    activeQuestion = 0;
    isModalOpen = false;
    quiz = getQuiz();
  }

  function setScore() {
    score = score + 1;
  }

  // Reactive Statement
  $: if (score > 1) {
    isModalOpen = true;
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
  <button on:click|once={handleStartNewQuizClick}>Start New Quiz</button>

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

{#if isModalOpen}
  <Modal on:close={resetQuiz}>
    <div slot="modal">
      <h2>You won!</h2>
      <p>Congratulations</p>
      <button on:click={resetQuiz}>Start Over</button>
    </div>
  </Modal>
{/if}
