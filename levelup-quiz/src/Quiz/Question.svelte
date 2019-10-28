<script>
  export let details;

  function handleAnswerSelect({ correctAnswer, givenAnswer }) {
    console.log(correctAnswer, givenAnswer);
  }
</script>

<style>
  .question {
    margin-bottom: 1rem;
    padding: 1rem;
    border: 1px solid #eee;
    box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.05);
  }

  .question__answers {
    display: flex;
  }

  .question__answers button {
    margin-right: 1rem;
    padding: 1rem;

    color: #333;
    font-weight: bold;
    text-transform: uppercase;

    background-color: transparent;
    border: 1px solid #d64c0c;
    cursor: pointer;
    transition: all 150ms ease-in-out;
  }

  .question__answers button:hover {
    color: #fff;

    background-color: #333;
    border: 1px solid #333;
  }

  .question__answers button:last-of-type {
    margin-right: 0;
  }
</style>

<div class="question">
  <h3>
    {@html details.question}
  </h3>
  <div class="question__answers">
    <button
      on:click={handleAnswerSelect.bind(this, {
        correctAnswer: details.correct_answer,
        givenAnswer: details.correct_answer
      })}>
      {@html details.correct_answer}
    </button>
    {#each details.incorrect_answers as answer}
      <button
        on:click={handleAnswerSelect.bind(this, {
          correctAnswer: details.correct_answer,
          givenAnswer: answer
        })}>
        {@html answer}
      </button>
    {/each}
  </div>
</div>
