<script>
  import { onMount } from "svelte";
  import P5canvas from "./P5canvas.svelte";
  import Chat from "./Chat.svelte";
  let text = "Now Sqetch & Guess the word!!";
  let headerText = text;
</script>

<main>
  <h1 class="typewriter">&gt;0&lt; {headerText}</h1>
  <div class="container">
    <P5canvas />
    <Chat />
  </div>
</main>

<svelte:head>
  <title>{text}</title>
  <meta name="description" content={text} />
</svelte:head>

<style>
  main {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 16px;
  }
  .container {
    display: flex;
    gap: 8px;
    width: 100%;
    min-width: 300px;
    max-width: 800px;
    padding: 16px;
  }
</style>
