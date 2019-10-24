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
