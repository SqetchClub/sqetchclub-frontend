<script>
  import P5canvas from './P5canvas.svelte';
</script>

<main>
  <h1>P5canvas with Svelte</h1>
  <P5canvas />
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
</style>

<svelte:head>
  <title>P5canvas with Svelte</title>
  <meta name="description" content="P5canvas with Svelte" />
</svelte:head>
