# Svelte For Beginners by Level Up Tutorials

[Svelte For Beginners](1) | [Svelte Documentation](2)

## 01: What Is Svelte

A JavaScript Framework For Interactive, Complex Web Applications

- JavaScript Frontend Framework (Can be server-side rendered)
- It compiles its code into vanilla JavaScript

## 02: Getting Started

```bash
npx degit sveltejs/template my-project-name
cd my-project-name

npm install
npm run dev
```

## 03: Starting Code

Reviewed the code that comes in the build

## 04: Our First Component & Props

```svelte
<script>
  import Container from "./Global/Container.svelte";
  import Header from "./Header.svelte";
  import Quiz from "./Quiz.svelte";
  export let name = "sup";
</script>

<Header />
<main>
  <Container>
    <h1>Hello {name}!</h1>
  </Container>
  <Quiz quizName="Frank Quiz" />
</main>
```

```svelte
<script>
  import Container from "./Global/Container.svelte";
  export let quizName = "Scott Quiz";
</script>

<Container>
  <h2>{quizName}</h2>
</Container>
```

## 05: Inputs, Binding & Reactivity

```svelte
<script>
  import Container from "./Global/Container.svelte";
  export let quizName = "Scott Quiz";
  let title = "";
  let a = 0;
  let b = 0;
</script>

<Container>
  <h2>{quizName}</h2>
  <h3>{title}</h3>
  <input bind:value={title} type="text" />
  <hr />
  <h4>{a + b}</h4>
  <input type="number" name="a" id="a" bind:value={a} />
  <input type="number" name="b" id="b" bind:value={b} />
</Container>

```

## 06: Basic Events

```svelte
<script>
  import Container from "./Global/Container.svelte";

  let result = "";
  let correctAnswer = "b";

  function handleButtonClick(input) {
    pickAnswer(input);
  }

  function pickAnswer(value) {
    result = value === correctAnswer ? "Correct" : "WRONG!";
  }
</script>

<Container>
  <h4>{result}</h4>
  <button on:click={handleButtonClick.bind(this, 'a')}>Answer A</button>
  <button on:click={handleButtonClick.bind(this, 'b')}>Answer B</button>
  <button on:click={handleButtonClick.bind(this, 'c')}>Answer C</button>
  <button on:click={handleButtonClick.bind(this, 'd')}>Answer D</button>
</Container>
```

## 07: Conditionals & Loops

```svelte
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
```

## 08: CSS In Svelte

- Scoped by default

To do global...

```svelte
<style>
  :global(h4) {
    color: blue;
  }
</style>
```

## 09: Hitting an API

```svelte
<script>
  import Container from "./Global/Container.svelte";

  let result = "";
  let correctAnswer = "b";
  let answers = ["a", "b", "c", "d"];
  let quiz = getQuiz();

  function handleButtonClick(input) {
    pickAnswer(input);
  }

  function pickAnswer(value) {
    result = value === correctAnswer ? "Correct" : "WRONG!";
  }

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=12&type=multiple"
    );
    const quiz = await res.json();
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
  <button on:click={getQuiz}>Get Quiz</button>
</Container>

```

## 10: The Await Tag

```svelte
    {#await quiz}
      Loading...
    {:then data}
      {data.results[0].question}
    {/await}
```

## 11: Our Questions

```svelte
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
```

```svelte
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
```

## 12: Getting The Quiz Working

```svelte
<script>
  export let details;

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
  }

  // Actions

  shuffle(allAnswers);
</script>

<div class="question">
  <h3>
    {@html details.question}
  </h3>
  {#if isAnswered}
    {#if isCorrect}
      <h4>Correct!</h4>
    {:else}
      <h4>WRONG!</h4>
    {/if}
  {/if}
  <div class="question__answers">
    {#each allAnswers as answer}
      <button
        on:click={handleAnswerSelect.bind(this, { correct: answer.correct })}>
        {@html answer.answer}
      </button>
    {/each}
  </div>
</div>

```

## 13: Improving The Interface

```svelte
<script>
  import Container from "./Global/Container.svelte";
  import Question from "./Quiz/Question.svelte";

  let activeQuestion = 0;
  let score = 0;
  let quiz = getQuiz();

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
  }

  function setScore() {
    score = score + 1;
  }
</script>

<Container>
  <h2>Take The Quiz!</h2>
  <h3>Current Score: {score} | Question {activeQuestion + 1}</h3>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <Question
          details={question}
          {nextQuestion}
          {setScore}
          {activeQuestion} />
      {/if}
    {/each}
  {/await}

  <button on:click={handleStartNewQuizClick}>Start New Quiz</button>
</Container>
```

## 14: Animations in Svelte

```svelte
<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
</script>
<div transition:fly={{ y: 100 }} class="fade-wrapper">
</div>
<div in:fly={{ y: 100 }} out:fly={{ y: -100 }} class="fade-wrapper">
</div>
```

[1]: https://www.leveluptutorials.com/tutorials/svelte-for-beginners/what-is-svelte
[2]: https://svelte.dev/
