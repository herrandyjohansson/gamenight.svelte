<script lang="ts">
  import { onMount } from "svelte";
  import { fade } from "svelte/transition"; // Import fade transition
  import Gamer from "./Gamer.svelte";

  const PLAYABLE_DAYS = ["MON", "TUE", "WED", "THU", "FRI"];

  // Different selected days for each player to simulate varied input
  const PLAYERS_SELECTED_DAYS_FROM_API = [
    ["MON", "FRI"], // Player 1
    ["TUE", "THU"], // Player 2
    ["WED"], // Player 3
    ["MON", "TUE", "WED", "THU", "FRI"], // Player 4 with all days selected
  ];

  let gamers: any = [
    {
      id: 1,
      name: "Player 1",
    },
    {
      id: 2,
      name: "Player 2",
    },
    {
      id: 3,
      name: "Player 3",
    },
  ];
  let loading = true;
  let error: Error | null = null;

  // Fetch gamers from API
  // async function fetchGamers() {
  //   try {
  //     const response = await fetch(
  //       "https://gamenight-fastapi.vercel.app/gamers",
  //       {
  //         headers: {
  //           "Content-Type": "application/json",
  //         },
  //       }
  //     );
  //     if (!response.ok) throw new Error("Failed to fetch gamers");

  //     const data = await response.json();
  //     gamers = data;
  //   } catch (err) {
  //     error = err as Error;
  //   } finally {
  //     loading = false;
  //   }
  // }

  // onMount(fetchGamers);
</script>

<section class="gamers" in:fade={{ duration: 11600 }}>
  <!-- {#if loading}
    <p>Loading gamers...</p>
  {:else if error}
    <p>Error loading gamers: {error.message}</p>
  {:else} -->
  {#each gamers as gamer, index}
    <div>
      <!-- Apply fade with a custom duration -->
      <Gamer
        {gamer}
        playableDays={PLAYABLE_DAYS}
        selectedDaysFromApi={PLAYERS_SELECTED_DAYS_FROM_API[index] || []}
        {index}
      />
    </div>
  {/each}
  <!-- {/if} -->
</section>

<style>
  .gamers {
    display: grid;
    grid-template-columns: repeat(3, minmax(50px, 1fr));
    grid-auto-rows: 1fr;
    flex-wrap: wrap;
    gap: 3rem;
  }
</style>
