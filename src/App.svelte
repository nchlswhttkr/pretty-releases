<script>
  import Home from "./Home.svelte";
  import ViewReleases from "./ViewReleases.svelte";
  import NotFound from "./NotFound.svelte";

  let path = window.location.pathname;
  let page = 1;

  function updatePath() {
    path = window.location.pathname;
  }

  function nav(newPath) {
    window.history.pushState({ page: page }, "Pretty Releases", newPath);
    page++;
    updatePath();
  }
</script>

<style>
  :global(body) {
    padding: 0;
    background-color: #ddd;
  }

  .nav > p {
    font-weight: 700;
    margin: 16px;
  }

  .main {
    box-sizing: border-box;
    padding: 0 16px 16px;
  }
</style>

<svelte:window on:popstate={updatePath} />

{#if path !== '/'}
  <nav class="nav">
    <p>
      <a on:click|preventDefault={() => nav('/')} href="/">{'< Home'}</a>
    </p>
  </nav>
{/if}
<main class="main">
  {#if path === '/'}
    <Home {nav} />
  {:else if path.startsWith('/releases/')}
    <ViewReleases />
  {:else}
    <NotFound {nav} />
  {/if}
</main>
