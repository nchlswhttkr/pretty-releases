<script>
  import marked from "marked";

  async function getReleases() {
    const repo = window.location.pathname.substring(10); // it works...
    return await fetch(`https://api.github.com/repos/${repo}/releases`)
      .then(response => {
        if (!response.ok) {
          throw new Error(`Received ${response.status} from GitHub`);
        }
        return response;
      })
      .then(response => response.json())
      .then(payload => {
        return payload.map(item => ({ ...item, body: marked(item.body) }));
      });
  }
  let promise = getReleases();
</script>

<style>
  .root {
    padding: 16px;
  }
  .release {
    background-color: #fff;
    padding: 20px;
    border: 2px solid black;
  }
  .splitter {
    height: 16px;
  }
  .error {
    color: red;
  }
</style>

<div class="root">
  {#await promise}
    <p>Loading...</p>
  {:then releases}
    {#each releases as release, i (release.id)}
      {#if i > 0}
        <div class="splitter" aria-hidden="true" />
      {/if}
      <article class="release">
        <h2>
          <a href={release.html_url}>{release.name}</a>
        </h2>
        <p>
          <em>
            Released by
            <a href={release.author.html_url}>{release.author.login}</a>
            on {release.created_at.split('T')[0]}
          </em>
        </p>
        {@html release.body}
      </article>
    {/each}
  {:catch error}
    <p class="error">{error}</p>
  {/await}
</div>
