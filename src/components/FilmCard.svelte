<script>
import sanitizeHtml from "sanitize-html";

export let name;
export let year;
export let rating;
export let rewatch;
export let url;
export let poster_url;

const stars = {
  "1.0": "★",
  "1.5": "★½",
  "2.0": "★★",
  "2.5": "★★½",
  "3.0": "★★★",
  "3.5": "★★★½",
  "4.0": "★★★★",
  "4.5": "★★★★½",
  "5.0": "★★★★★",
};
</script>

<span class="film">
  <a href={url}>
    <img class="poster" src={poster_url} width="150" height="225" alt={name} />
  </a>
  <a href={url}><p class="film-name">{sanitizeHtml(name)}</p></a>
  <p class="film-year">{year}</p>
  <p class="film-rating">
    {#if rewatch}
      🔄
    {/if}
    {stars[rating]}
  </p>
</span>

<style lang="scss">
.film {
  display: flex;
  flex-direction: column;
  max-width: 150px;
  text-align: center;
  align-items: center;
}

.film-name {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  overflow: hidden;
  text-overflow: ellipsis;

  padding-top: 0.5rem;
  font-weight: 600;
  font-size: 0.9rem;
  text-transform: uppercase;
}

.film-year {
  color: var(--site-secondary-text-color);
  font-weight: 400;
  font-size: 0.8rem;
}

.film-rating {
  color: var(--site-accent-color);
}

.poster {
  border-radius: 8px;
}

a {
  color: var(--site-primary-text-color);

  &:hover,
  &:focus {
    color: var(--site-primary-text-color);
  }
}
</style>
