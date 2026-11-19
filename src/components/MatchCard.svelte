<script>
export let hero;
export let hero_image;
export let kills;
export let deaths;
export let assists;
export let won;
export let url;
export let time_ago;
export let duration;
export let items = null;

let imageFailed = false;
</script>

<a
  class="match"
  class:won
  href={url}
  target="_blank"
  rel="noopener noreferrer"
  title={won ? "Won" : "Lost"}
>
  {#if hero_image && !imageFailed}
    <img
      class="hero-image"
      src={hero_image}
      width="57"
      height="32"
      alt="{hero} — Hero Portrait"
      loading="lazy"
      on:error={() => (imageFailed = true)}
    />
  {:else}
    <span class="hero-image placeholder"></span>
  {/if}
  <span class="hero">
    <span class="hero-name">{hero}</span>
    <span class="time-ago" title="Played">{time_ago}</span>
  </span>
  {#if items?.some(Boolean)}
    <span class="items">
      {#each items as item, i}
        {#if item}
          <img
            class="item"
            src={item.image}
            width="33"
            height="24"
            alt={item.name}
            title={item.name}
            loading="lazy"
            on:error={() => (items[i] = null)}
          />
        {/if}
      {/each}
    </span>
  {/if}
  <span class="kda">
    {kills} / <span class="deaths">{deaths}</span> / {assists}
  </span>
  <span class="result">{won ? "W" : "L"}</span>
  <span class="duration" title="Match duration">
    {duration}
  </span>
</a>

<style lang="scss">
.match {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 12px;

  padding: 0.35rem 0.75rem 0.35rem 0.75rem;
  border-left: 3px solid var(--site-red-color);
  color: var(--site-primary-text-color);

  &.won {
    border-left-color: var(--site-accent-color);
  }

  &:hover {
    color: var(--site-primary-text-color);
  }
}

.hero-image {
  flex-shrink: 0;
  width: 57px;
  height: 32px;
  border-radius: 4px;
  object-fit: cover;

  &.placeholder {
    background-color: var(--site-secondary-color);
  }
}

.hero {
  display: flex;
  flex: 1;
  align-items: baseline;
  gap: 8px;
  min-width: 0;
}

.hero-name {
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  font-weight: 600;
  font-size: 0.9rem;
  text-transform: uppercase;
}

.items {
  display: flex;
  flex-shrink: 0;
  gap: 3px;
}

.item {
  width: 33px;
  height: 24px;
  border-radius: 3px;
  object-fit: cover;
}

.duration,
.time-ago {
  flex-shrink: 0;
  color: var(--site-secondary-text-color);
  font-weight: 400;
  font-size: 0.8rem;
  font-variant-numeric: tabular-nums;
  white-space: nowrap;
}

.duration {
  min-width: 2.5rem;
  text-align: left;
}

.kda {
  min-width: 3.75rem;
  text-align: right;
  font-size: 0.9rem;
  font-variant-numeric: tabular-nums;
  white-space: nowrap;
}

.deaths {
  color: var(--site-red-color);
}

.result {
  width: 0.9rem;
  text-align: center;
  font-weight: 600;
  font-size: 0.8rem;
  color: var(--site-red-color);

  .won & {
    color: var(--site-accent-color);
  }
}

@media (max-width: 600px) {
  .match {
    gap: 8px;
    padding-left: 0.5rem;
    padding-right: 0.5rem;
  }

  .hero {
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    line-height: 1.15rem;
  }

  .items {
    display: grid;
    grid-template-columns: repeat(3, 33px);
  }
}
</style>
