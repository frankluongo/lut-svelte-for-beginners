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
