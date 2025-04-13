<script>
import FilmCard from "@components/FilmCard.svelte";
import { onMount } from "svelte";

const username = "fawwn";

let filmsData;

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
      const posterUrl = description
        ? description.match(/<img src="([^"]+)"/)?.[1]
        : "";

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
  }
};

onMount(async () => await fetchFilms());
</script>

<!-- TODO: you know... actual data... -->
<div class="films">
  {#each filmsData as data}
    {#if data}
      <FilmCard {...data} />
    {/if}
  {/each}
</div>
<div style="color: var(--site-secondary-text-color); margin-bottom: 1.5rem">
  → More on <a href="https://letterboxd.com/fawwn">Letterboxd</a>
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
