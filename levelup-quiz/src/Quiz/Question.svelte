<script>
  import { score } from "../Store.js";
  export let activeQuestion;
  export let details;
  export let nextQuestion;

  let answers = details.incorrect_answers.map(answer => {
    return {
      answer,
      correct: false
    };
  });
  let allAnswers = [
    ...answers,
    {
      answer: details.correct_answer,
      correct: true
    }
  ];
  let isCorrect;
  let isAnswered = false;

  function handleAnswerSelect({ correct }) {
    checkQuestion(correct);
  }

  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  function checkQuestion(correct) {
    isCorrect = correct;
    isAnswered = true;
    if (isCorrect) {
      score.update(val => val + 1);
    }
  }

  // Actions

  shuffle(allAnswers);
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

  .question__answers button[disabled] {
    color: #ccc;

    background-color: #eee;
    border: 1px solid #eee;
  }

  .question__answers button:last-of-type {
    margin-right: 0;
  }
</style>

<div class="question">
  <h3>
    {@html details.question}
  </h3>
  {#if isAnswered}
    <!-- <h4 class={isCorrect ? 'correct' : 'wrong'}> -->
    <!-- <h4 class:correct={isCorrect}> -->
    <!-- <h4 class:isCorrect> -->
    <h4 class:isCorrect class:wrong={!isCorrect}>
      {#if isCorrect}Correct!{:else}WRONG!{/if}
    </h4>
  {/if}
  <div class="question__answers">
    {#each allAnswers as answer}
      <button
        on:click={handleAnswerSelect.bind(this, { correct: answer.correct })}
        disabled={isAnswered}>
        {@html answer.answer}
      </button>
    {/each}
  </div>
  {#if isAnswered && activeQuestion < 9}
    <div class="question__next">
      <button on:click={nextQuestion}>Next Question</button>
    </div>
  {/if}
</div>
