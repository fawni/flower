<script>
export let name;
export let artist;
export let album;
export let url;
export let artist_url;
export let cover_url;
export let loved;
</script>

<span class="track">
  <a href={url} target="_blank" rel="noopener noreferrer">
    <img
      class="cover"
      src={cover_url}
      width="150"
      height="150"
      alt="{album} — Album Cover"
      title="{album} — Album Cover"
    />
  </a>
  <a
    href={url}
    title={name}
    target="_blank"
    rel="noopener noreferrer"
  ><p class="track-name">{name}</p></a>
  <a
    href={artist_url}
    title={artist}
    target="_blank"
    rel="noopener noreferrer"
  ><p class="track-artist">{artist}</p></a>
  {#if loved}<p style="color: var(--site-accent-color); font-weight: 500; font-size: 0.9rem">
      ❤
    </p>{/if}
</span>

<style lang="scss">
.track {
  display: flex;
  flex-direction: column;
  max-width: 150px;
  text-align: center;
  align-items: center;
  line-height: 1.25rem;
}

.track-name {
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

.track-artist {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 1;
  line-clamp: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;

  color: var(--site-secondary-text-color);
  font-weight: 400;
  font-size: 0.8rem;
  text-transform: uppercase;
}

.cover {
  border-radius: 8px;
}

a {
  color: var(--site-primary-text-color);

  &:hover {
    color: var(--site-primary-text-color);
  }
}
</style>
