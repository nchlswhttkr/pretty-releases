<script>
  export let nav;

  let input = "";

  function submit() {
    // TODO: Enforce validation on this field with feedback/errors of sorts
    // TODO: Strip trailing text (subpath, query, hash)
    let repo = input.slice(19);
    nav("/releases/" + repo);
  }
</script>

<style>
  .outer {
    width: 100%;
    height: 100vh;
    max-height: 800px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .titles {
    text-align: center;
  }

  .inner {
    margin-top: 40px;
    display: flex;
    flex-direction: column;

    position: relative;
    animation: float-up 0.4s ease-out 0.2s forwards;
    opacity: 0;
  }

  .inner > * {
    margin: 8px 0;
  }

  .inner > label {
    display: none;
  }

  .inner > input {
    display: block;
    width: 400px;
    max-width: 90vw;
  }

  .inner > button {
    align-self: flex-end;
  }

  @keyframes float-up {
    0% {
      top: 20vh;
      opacity: 0;
    }
    100% {
      top: 0;
      opacity: 1;
    }
  }
</style>

<div class="outer">
  <h1 class="titles">Pretty Releases</h1>
  <h2 class="titles">
    View releases for a
    <a href="https://github.com" target="_blank" rel="noopener">GitHub</a>
    repository
  </h2>
  <div class="inner">
    <label for="repo-name">Repository name</label>
    <input
      id="repo-name"
      bind:value={input}
      placeholder="https://github.com/electron/electron" />
    <button on:click={submit}>Submit</button>
  </div>
</div>
