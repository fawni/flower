<script>
import { onMount } from "svelte";

import Loading from "@components/Loading.svelte";
import MatchCard from "./MatchCard.svelte";

const account_id = "846970755";
const api = "https://api.opendota.com/api";
const hero_image_base =
  "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/heroes";
const item_image_base =
  "https://cdn.cloudflare.steamstatic.com/apps/dota2/images/dota_react/items";

let matchesData = 0;

const timeAgo = (start_time) => {
  const seconds = Math.max(0, Date.now() / 1000 - start_time);
  const units = [
    ["d", 86400],
    ["h", 3600],
    ["m", 60],
  ];

  for (const [label, size] of units) {
    if (seconds >= size) return `${Math.floor(seconds / size)}${label} ago`;
  }
  return "just now";
};

const formatDuration = (total) => {
  const hours = Math.floor(total / 3600);
  const minutes = Math.floor((total % 3600) / 60);
  const seconds = String(total % 60).padStart(2, "0");

  return hours
    ? `${hours}:${String(minutes).padStart(2, "0")}:${seconds}`
    : `${minutes}:${seconds}`;
};

const fetchJson = async (url) => {
  try {
    const response = await fetch(url);
    return response.ok ? await response.json() : null;
  } catch {
    return null;
  }
};

const fetchItems = async (matches) => {
  const [itemIds, details] = await Promise.all([
    fetchJson(`${api}/constants/item_ids`),
    Promise.all(
      matches.map((match) => fetchJson(`${api}/matches/${match.match_id}`)),
    ),
  ]);

  matchesData = matchesData.map((data, i) => {
    const player = details[i]?.players?.find(
      (p) => p.player_slot === matches[i].player_slot,
    );
    if (!player) return data;

    const items = itemIds
      ? [0, 1, 2, 3, 4, 5].map((slot) => {
        const id = player[`item_${slot}`];
        const name = id ? itemIds[id] : null;
        return name
          ? {
            name: name.replaceAll("_", " "),
            image: `${item_image_base}/${name}.png`,
          }
          : null;
      })
      : null;

    return { ...data, items };
  });
};

const fetchMatches = async () => {
  try {
    const [matchesResponse, heroesResponse] = await Promise.all([
      fetch(`${api}/players/${account_id}/recentMatches`),
      fetch(`${api}/heroes`),
    ]);
    if (!matchesResponse.ok || !heroesResponse.ok) {
      throw new Error("OpenDota response was not ok");
    }

    const matches = await matchesResponse.json();
    const heroes = await heroesResponse.json();

    const heroesById = new Map(heroes.map((hero) => [hero.id, hero]));

    const recent = matches.slice(0, 5);

    matchesData = recent.map((match) => {
      const hero = heroesById.get(match.hero_id);
      const radiant = match.player_slot < 128;

      return {
        hero: hero?.localized_name ?? "Unknown Hero",
        hero_image: hero
          ? `${hero_image_base}/${hero.name.replace("npc_dota_hero_", "")}.png`
          : null,
        kills: match.kills,
        deaths: match.deaths,
        assists: match.assists,
        won: radiant === match.radiant_win,
        url: `https://www.dotabuff.com/matches/${match.match_id}`,
        time_ago: timeAgo(match.start_time),
        duration: formatDuration(match.duration),
        items: null,
      };
    });

    if (recent.length) fetchItems(recent);
  } catch (error) {
    console.error("Error fetching matches:", error);
    matchesData = null;
    return;
  }
};

onMount(async () => await fetchMatches());
</script>

<div class="matches">
  {#if matchesData === 0}
    <Loading />
  {:else if matchesData === null}
    <blockquote class="error">
      Error! OpenDota likely down. <a
        href="https://www.opendota.com/"
        target="_blank"
        rel="noopener noreferrer"
      >Service status</a>.
    </blockquote>
  {:else if matchesData.length === 0}
    <blockquote class="error">
      No public matches found.
    </blockquote>
  {:else}
    {#each matchesData as data}
      <MatchCard {...data} />
    {/each}
  {/if}
</div>
<div style="color: var(--site-secondary-text-color); margin-bottom: 1.5rem">
  → More on <a
    href="https://www.dotabuff.com/players/{account_id}"
    target="_blank"
    rel="noopener noreferrer"
  >Dotabuff</a>
</div>

<style lang="scss">
.matches {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding-bottom: 0.5rem;
  margin-bottom: 0.5rem;

  @media (max-width: 850px) {
    min-width: 500px;
  }
}
</style>
