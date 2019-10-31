<script>
  import Container from "./Global/Container.svelte";
  import Header from "./Header.svelte";
  import Quiz from "./Quiz.svelte";
  export let name = "sup";
</script>

<style>
  :global(*),
  :global(*:before),
  :global(*):after {
    box-sizing: border-box;
  }
</style>

<Header />
<main>
  <Container>
    <h1>THE Music Quiz</h1>
    <Quiz quizName="Frank Quiz" />
  </Container>
</main>
