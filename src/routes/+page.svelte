<script>
  import { onMount } from "svelte";
  import Entrance from "./Entrance.svelte";

  let text = "Welcome to SqetchClub!!";
  let welcomeText = "";
  let index = 0;

  function startTyping() {
    welcomeText = "";
    index = 0;
    const interval = setInterval(() => {
      if (index < text.length) {
        welcomeText += text[index];
        index++;
      } else {
        clearInterval(interval);
        setTimeout(startTyping, 8000); 
      }
    }, 150); 
  }

  onMount(() => {
    startTyping();
  });
</script>

<svelte:head>
  <title>{text}</title>
  <meta name="description" content={text} />
</svelte:head>

<section>
  <h1>&gt;_&lt;<br /><span class="typewriter">{welcomeText}</span></h1>
  <Entrance />
</section>

<style>
  section {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 16px;
  }

  .typewriter {
    display: inline-block;
    overflow: hidden;
    border-right: 3px solid black; /* Cursor effect */
    white-space: nowrap;
    animation: blink-caret 0.75s step-end infinite;
  }

  @keyframes blink-caret {
    from,
    to {
      border-color: transparent;
    }
    50% {
      border-color: black;
    }
  }
</style>
