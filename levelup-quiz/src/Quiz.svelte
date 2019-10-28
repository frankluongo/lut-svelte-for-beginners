<script>
  import Container from "./Global/Container.svelte";
  import Question from "./Quiz/Question.svelte";
  let quiz = getQuiz();

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=12&type=multiple"
    );
    return await res.json();
  }

  function handleClick() {
    quiz = getQuiz();
  }

  function handleAnswerSelect({ correctAnswer, givenAnswer }) {
    console.log(correctAnswer, givenAnswer);
  }
</script>

<Container>

  <h2>Take The Quiz!</h2>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question}
      <Question details={question} />
    {/each}
  {/await}

  <button on:click={handleClick}>Get Quiz</button>
</Container>
