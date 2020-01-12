<script>
  export let nav;

  let input = "";

  function submit() {
    nav("/releases/" + input);
  }
</script>

<h1>Home</h1>
<input bind:value={input} placeholder="electron/electron" />
<button on:click={submit}>Submit</button>
