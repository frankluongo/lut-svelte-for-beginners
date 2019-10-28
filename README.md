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

[1]: https://www.leveluptutorials.com/tutorials/svelte-for-beginners/what-is-svelte
[2]: https://svelte.dev/
