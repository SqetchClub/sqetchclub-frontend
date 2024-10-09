<script>
  import Header from "./Header.svelte";
  import "../app.css";
</script>

<div class="app">
  <Header />
  <main>
    <slot />
  </main>
  <footer>
    <p>
      copyright <a href="https://github.com/SqetchClub">SqetchClub</a> 2024
    </p>
  </footer>
</div>

<style>
  .app {
    display: flex;
    flex-direction: column;
    height: 100vh;
    min-height: 100vh;
    overflow: hidden;
  }

  main {
    height: 100%;
  }

  footer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 12px;
    border-top: 1px dashed;
  }

  footer a {
    font-weight: bold;
  }
</style>
