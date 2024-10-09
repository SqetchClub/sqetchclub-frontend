<script>
  import { page } from "$app/stores";
</script>

<header>
  <nav>
    <ul>
      <li aria-current={$page.url.pathname === "/" ? "page" : undefined}>
        <a href="/">Lobby</a>
      </li>
      <li
        aria-current={$page.url.pathname.startsWith("/p5canvas")
          ? "page"
          : undefined}
      >
        <a href="/p5canvas">Club</a>
      </li>
    </ul>
  </nav>
</header>

<style>
  header {
    display: flex;
    justify-content: space-between;
	border-bottom: 1px dashed;
  }

  nav {
    width: 100%;
	padding: 12px;
  }

  ul {
    display: flex;
    justify-content: center;
    gap: 16px;
  }
</style>
