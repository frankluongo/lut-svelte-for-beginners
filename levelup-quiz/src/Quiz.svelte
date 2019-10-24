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
