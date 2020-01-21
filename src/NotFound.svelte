<script>
  import { onDestroy } from "svelte";

  export let nav;

  const timeoutId = setTimeout(() => {
    nav("/");
  }, 2000);

  onDestroy(() => {
    // Cancels navigation if the page is left (like clicking the home link)
    clearTimeout(timeoutId);
  });
</script>

<h1>Page not found</h1>
<p>You will be redirected momentarily.</p>
