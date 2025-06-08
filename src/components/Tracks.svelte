<script>
import { onMount } from "svelte";

import Loading from "@components/Loading.svelte";
import TrackCard from "./TrackCard.svelte";

const username = "aokiare";
const api_key = import.meta.env.PUBLIC_LASTFM_API_KEY;

let tracksData = 0;

const fetchTracks = async () => {
  try {
    const response = await fetch(
      `https://ws.audioscrobbler.com/2.0/?method=user.getrecenttracks&user=${username}&api_key=${api_key}&format=json&limit=5&extended=1`,
    );
    const data = await response.json();
    const tracks = data?.recenttracks?.track?.slice(0, 5) || [];

    tracksData = tracks.map((track) => ({
      name: track?.name,
      artist: track?.artist.name,
      album: track?.album["#text"],
      url: track?.url,
      artist_url: track?.artist.url,
      cover_url: track?.image[3]["#text"].replace(/\/\d{1,3}x\d{1,3}\//, "/") || "/hi.jpg",
      loved: track?.loved === "1" ? true : false,
    }));
  } catch (error) {
    console.error("Error fetching tracks:", error);
    tracksData = null;
    return;
  }
};

onMount(async () => await fetchTracks());
</script>

<div class="tracks">
  {#if tracksData}
    {#each tracksData as data}
      {#if data}
        <TrackCard {...data} />
      {/if}
    {/each}
  {:else if tracksData === 0}
    <Loading />
  {:else}
    <blockquote class="error">
      Error! Last.fm likely down. <a
        href="https://status.last.fm/"
        target="_blank"
        rel="noopener noreferrer"
      >Service status</a>.
    </blockquote>
  {/if}
</div>
<div style="color: var(--site-secondary-text-color); margin-bottom: 1.5rem">
  → More on <a
    href="https://last.fm/user/Aokiare"
    target="_blank"
    rel="noopener noreferrer"
  >Last.fm</a>
</div>

<style lang="scss">
.tracks {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  gap: 20px;
  padding-bottom: 0.5rem;
  margin-bottom: 0.5rem;

  @media (max-width: 850px) {
    flex-wrap: nowrap;
    overflow-x: auto;
    min-width: 500px;
  }
}
</style>
