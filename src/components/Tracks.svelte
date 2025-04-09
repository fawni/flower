<script>
import { onMount } from "svelte";
import TrackCard from "./TrackCard.svelte";

const username = "aokiare";
const api_key = import.meta.env.PUBLIC_LASTFM_API_KEY;

let tracksData;

const fetchTracks = async () => {
  try {
    const response = await fetch(
      `https://ws.audioscrobbler.com/2.0/?method=user.getrecenttracks&user=${username}&api_key=${api_key}&format=json`,
    );
    const data = await response.json();
    const tracks = data?.recenttracks?.track?.slice(0, 5) || [];

    tracksData = tracks.map((track) => ({
      name: track?.name,
      artist: track?.artist["#text"],
      album: track?.album["#text"],
      url: track?.url,
      cover_url: track?.image[3]["#text"],
    }));
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

onMount(async () => await fetchTracks());
</script>

<div class="tracks">
  {#each tracksData as data}
    {#if data}
      <TrackCard {...data} />
    {/if}
  {/each}
</div>
<div style="color: var(--site-secondary-text-color); margin-bottom: 1.25rem">
  → More on <a href="https://last.fm/user/Aokiare">Last.fm</a>
</div>

<style>
.tracks {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin-bottom: 0.5rem;
}
</style>
