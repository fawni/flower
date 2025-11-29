<script>
import { onMount } from "svelte";

import FilmCard from "@components/FilmCard.svelte";
import Loading from "@components/Loading.svelte";

const username = "fawwn";

let filmsData = 0;

const fetchFilms = async () => {
  try {
    const response = await fetch(`https://boxed.fawn.moe/${username}`);
    if (!response.ok) {
      throw new Error("Boxed response was not ok");
    }

    const text = await response.text();
    const parser = new DOMParser();
    const xmlDoc = parser.parseFromString(text, "text/xml");

    const films = [];
    const items = xmlDoc.getElementsByTagName("item");

    for (let i = 0; i < 5; i++) {
      const item = items[i];

      const getTextContent = (tagName) => {
        const element = item.getElementsByTagName(tagName)[0];
        return element ? element.textContent : null;
      };

      const name = getTextContent("letterboxd:filmTitle");
      const year = getTextContent("letterboxd:filmYear");
      const rating = getTextContent("letterboxd:memberRating");
      const rewatch = getTextContent("letterboxd:rewatch") === "Yes";

      const url = getTextContent("link");

      const description = getTextContent("description");
      const posterUrl = description.match(/<img src="([^"]+)"/)?.[1]
        ?? "https://s.ltrbxd.com/static/img/empty-poster-1000-D9cprv0m.png";

      films.push({
        name: name,
        year: year,
        rating: rating,
        rewatch: rewatch,
        url: url,
        poster_url: posterUrl,
      });
    }

    filmsData = films;
  } catch (error) {
    console.error("Error fetching films:", error);
    filmsData = null;
    return;
  }
};

onMount(async () => await fetchFilms());
</script>

<!-- TODO: you know... actual data... -->
<div class="films">
  {#if filmsData}
    {#each filmsData as data}
      {#if data}
        <FilmCard {...data} />
      {/if}
    {/each}
  {:else if filmsData === 0}
    <Loading />
  {:else}
    <blockquote class="error">
      Error! Letterboxd likely down. <a
        href="https://bsky.app/profile/letterboxd.social"
        target="_blank"
        rel="noopener noreferrer"
      >Service status</a>.
    </blockquote>
  {/if}
</div>
<div style="color: var(--site-secondary-text-color); margin-bottom: 1.5rem">
  → More on <a
    href="https://letterboxd.com/fawwn"
    target="_blank"
    rel="noopener noreferrer"
  >Letterboxd</a>
</div>

<style lang="scss">
.films {
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
