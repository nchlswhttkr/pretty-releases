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
</style>

<svelte:window on:popstate={updatePath} />

<main>
  {#if path == '/'}
    <Home {nav} />
  {:else if path.startsWith('/releases/')}
    <ViewReleases />
  {:else}
    <NotFound {nav} />
  {/if}
</main>
