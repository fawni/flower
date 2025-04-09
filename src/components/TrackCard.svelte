<script>
export let name;
export let artist;
export let album;
export let url;
export let cover_url;
</script>

<span class="track">
  <a href={url}>
    <img
      class="cover"
      src={cover_url}
      width="150"
      height="150"
      alt="{album} — Album Cover"
    />
  </a>
  <a href={url}><p class="track-name">{name}</p></a>
  <p class="track-artist">{artist}</p>
</span>

<style lang="scss">
.track {
  display: flex;
  flex-direction: column;
  max-width: 150px;
  text-align: center;
  align-items: center;
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

  &:hover,
  &:focus {
    color: var(--site-primary-text-color);
  }
}
</style>
