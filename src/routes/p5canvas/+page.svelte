<script>
  import P5canvas from "./P5canvas.svelte";
  import Chat from "./Chat.svelte";
</script>

<main>
  <h1>P5canvas with Svelte</h1>

  <div class="container">
    <P5canvas />
    <Chat />
  </div>
</main>

<svelte:head>
  <title>P5canvas with Svelte</title>
  <meta name="description" content="P5canvas with Svelte" />
</svelte:head>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .container {
    display: grid;
    grid-template-columns: 1fr auto;
    height:100%;
    gap: 20px;
  }
</style>
