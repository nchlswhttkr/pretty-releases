<script>
  import marked from "marked";

  let releases;
  let releaseIndex = -1; // -1 if none selected
  let selectedRelease = undefined;

  async function getReleases() {
    const repo = window.location.pathname.slice(10); // it works...
    releases = await fetch(`https://api.github.com/repos/${repo}/releases`)
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

  function showReleaseAtIndex(index) {
    releaseIndex = index;
    window.scroll(
      0,
      window.scrollY +
        document.getElementById("current-release").getBoundingClientRect().top -
        16
    );
  }

  $: selectedRelease = releases ? releases[releaseIndex] : undefined;

  let promise = getReleases();
</script>

<style>
  .release-container {
    display: flex;
    flex-wrap: wrap-reverse;
    max-width: 1200px;
    margin: 0 auto;
  }

  .release-list {
    flex: 1;
    min-width: 200px;
  }

  .release-viewer {
    box-sizing: border-box;
    flex: 3;
    min-width: 280px;
    padding: 8px 0;
    margin-bottom: 32px;
  }

  .release-viewer > article {
    box-sizing: border-box;
    margin: 0 auto;
    max-width: 600px;
    background-color: #fff;
    padding: 8px;
    border: 2px solid black;
    position: -webkit-sticky;
    position: sticky;
    top: 16px;
  }
  .release-viewer-body {
    overflow-wrap: anywhere;
  }

  .release-list > div {
    background-color: #fff;
    margin: 8px 16px 8px 0;
    padding: 8px;
    border: 2px solid black;
    transition: margin-right 0.2s;
  }

  .release-list > div.selected {
    margin-right: 0;
  }

  .release-list > div > * {
    margin: 4px 0;
  }

  .error {
    color: red;
  }
</style>

<div class="root">
  {#await promise}
    <p>Loading...</p>

  {:then _}
    <div class="release-container">

      <!-- THE LIST OF RELEASES -->
      <div class="release-list">
        {#each releases as release, i (release.id)}
          <div
            on:click={() => showReleaseAtIndex(i)}
            class:selected={i === releaseIndex}>
            <h3>
              <a href={release.html_url}>{release.name}</a>
            </h3>
            <p>
              <em>
                Released by
                <a href={release.author.html_url}>{release.author.login}</a>
                on {release.created_at.split('T')[0]}
              </em>
            </p>
          </div>
        {/each}
      </div>

      <!-- VIEW THE SELECTED RELEASE -->
      <div class="release-viewer">
        {#if selectedRelease !== undefined}
          <article id="current-release">
            <h3>
              <a href={selectedRelease.html_url}>{selectedRelease.name}</a>
            </h3>
            <p>
              <em>
                Released by
                <a href={selectedRelease.author.html_url}>
                  {selectedRelease.author.login}
                </a>
                on {selectedRelease.created_at.split('T')[0]}
              </em>
            </p>
            <div class="release-viewer-body">
              {@html selectedRelease.body}
            </div>
          </article>
        {/if}
      </div>

    </div>
  {:catch error}
    <p class="error">{error}</p>
  {/await}
</div>
