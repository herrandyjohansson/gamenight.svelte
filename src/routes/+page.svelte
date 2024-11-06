<script lang="ts">
  import { onMount } from "svelte";
  import Gamers from "./Gamers.svelte";

  let gamers: any = [];
  let loading = false;
  let error = null;

  async function fetchGamers() {
    try {
      const response = await fetch(
        "https://gamenight-fastapi.vercel.app/gamers",
        {
          headers: {
            "Content-Type": "application/json",
            "Access-Control-Allow-Origin": "*",
            mode: "no-cors",
          },
        }
      );
      if (!response.ok) throw new Error("Failed to fetch gamers");

      const data = await response.json();
      gamers = data;
    } catch (err) {
      // TODO - handle error
      error = err;
    } finally {
      loading = false;
    }
  }

  onMount(fetchGamers);
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
  <!-- {#if loading}
    <p>Loading...</p>
  {:else if gamers.length > 0}
    <ul>
      {#each gamers as gamer}
        <li>{gamer.name}</li>
      {/each}
    </ul>
  {:else}
    <p>No gamers found.</p>
  {/if} -->

  <!-- <Counter /> -->
  <Gamers />
</section>

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 0.6;
  }
</style>
