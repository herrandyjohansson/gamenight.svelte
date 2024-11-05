<script lang="ts">
  import Counter from "./Counter.svelte";
  import welcome from "$lib/images/svelte-welcome.webp";
  import welcomeFallback from "$lib/images/svelte-welcome.png";
  import { onMount } from "svelte";

  let gamers: any = [];
  let loading = false;
  let error = null;

  async function fetchGamers() {
    try {
      const response = await fetch(
        "https://gamenight-fastapi.vercel.app/gamers"
      );
      if (!response.ok) {
        throw new Error("Failed to fetch gamers");
      }
      const data = await response.json();
      gamers = data;
    } catch (err) {
      // TODO - handle error
      error = err;
    } finally {
      loading = false;
    }
  }

  // fetch gamers on mount
  onMount(fetchGamers);
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
  <h1>
    <span class="welcome">
      <picture>
        <source srcset={welcome} type="image/webp" />
        <img src={welcomeFallback} alt="Welcome" />
      </picture>
    </span>

    to your new<br /> GameNight app!
  </h1>

  {#if loading}
    <p>Loading...</p>
  {:else if gamers.length > 0}
    <ul>
      {#each gamers as gamer}
        <li>{gamer.name}</li>
      {/each}
    </ul>
  {:else}
    <p>No gamers found.</p>
  {/if}

  <h2>
    try editing <strong>src/routes/+page.svelte</strong>
  </h2>

  <Counter />
</section>

<style>
  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    flex: 0.6;
  }

  h1 {
    width: 100%;
  }

  .welcome {
    display: block;
    position: relative;
    width: 100%;
    height: 0;
    padding: 0 0 calc(100% * 495 / 2048) 0;
  }

  .welcome img {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    display: block;
  }
</style>
