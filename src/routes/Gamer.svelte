<script lang="ts">
  import { onMount } from "svelte";
  import FaSpinner from "svelte-fa";
  import { faSpinner } from "@fortawesome/free-solid-svg-icons";

  export let gamer: { id: number; name: string };
  export let playableDays: string[];
  export let index: number;
  export let selectedDaysFromApi: string[];

  let selectedDays = new Set<string>();
  let hasChanges = false; // Tracks if there are any changes
  let loading = false; // Tracks the loading state for API submission

  // Initialize selected days based on API data on mount
  onMount(() => {
    selectedDays = new Set(selectedDaysFromApi);
  });

  // Toggle day selection and check for changes
  function toggleDay(day: string) {
    if (selectedDays.has(day)) {
      selectedDays.delete(day); // Deselect if already selected
    } else {
      selectedDays.add(day); // Select if not already selected
    }

    selectedDays = new Set(selectedDays); // Trigger reactivity
    checkForChanges();
  }

  // Check if the current selectedDays set differs from the initial selectedDaysFromApi
  function checkForChanges() {
    hasChanges =
      selectedDays.size !== selectedDaysFromApi.length ||
      [...selectedDays].some((day) => !selectedDaysFromApi.includes(day)) ||
      selectedDaysFromApi.some((day) => !selectedDays.has(day));
  }

  // Prepare data for API submission with selected days as true and others as false
  function prepareDataForApi() {
    return {
      gamerId: gamer.id,
      playableDays: Object.fromEntries(
        playableDays.map((day) => [day, selectedDays.has(day)])
      ),
    };
  }

  // Submit to API
  async function submitToApi() {
    const weekData = prepareDataForApi();
    console.log("SEND TO API", weekData);

    loading = true; // Start loading animation

    // Simulate API update
    await new Promise((resolve) => setTimeout(resolve, 1200));

    console.log("API SUBMIT");
    selectedDaysFromApi = Array.from(selectedDays); // Update selectedDaysFromApi after submission
    hasChanges = false; // Reset changes after submission
    loading = false; // Stop loading animation
  }
</script>

<section class="gamers">
  <div class="gamers__player">
    <h3 class="gamers__player-title">P{index + 1}</h3>
    <div class="gamers__player-details">
      {#each playableDays as day}
        <button
          type="button"
          class="gamers__player-detail"
          class:green={selectedDaysFromApi.includes(day) &&
            selectedDays.has(day)}
          class:green-border={selectedDays.has(day) &&
            !selectedDaysFromApi.includes(day)}
          class:red-border={!selectedDays.has(day) &&
            selectedDaysFromApi.includes(day)}
          on:click={() => toggleDay(day)}
          aria-pressed={selectedDays.has(day)}
        >
          {day}
        </button>
      {/each}
    </div>
    <button
      class="gamers__submit-button"
      on:click={submitToApi}
      disabled={loading}
      class:loading
      class:gamers__submit-button--visible={hasChanges}
    >
      {#if loading}
        COOL
      {:else}
        DO IT
      {/if}
    </button>
  </div>
</section>

<style>
  .gamers__player-title {
    font-size: 2rem;
    display: flex;
    justify-content: center;
  }

  .gamers__player-details {
    display: flex;
    gap: 1rem;
    flex-direction: column;
    font-weight: 800;
  }

  .gamers__player-detail {
    background-color: black;
    color: white;
    border: 5px solid transparent;
    border-radius: 1rem;
    font-size: 1.5rem;
    display: flex;
    height: 4rem;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: border 0.3s ease-in-out;
  }

  /* Green background for API-selected days that are currently selected */
  .green {
    background-color: #28a745;
  }

  /* Green border for newly selected days not in API */
  .green-border {
    border: 5px solid #28a745;
  }

  /* Red border for deselected days that were initially in API */
  .red-border {
    border: 5px solid #dc3545;
  }

  .gamers__submit-button {
    opacity: 0;
    margin-top: 1rem;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 0.5rem;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-weight: bold;
    color: #fff;
    background-color: #28a745;
    transition:
      background-color 0.3s ease,
      opacity 0.3s ease;
    pointer-events: none; /* Prevents any interactions */
  }

  .gamers__submit-button--visible {
    opacity: 1;
    pointer-events: all;
  }

  .gamers__submit-button.loading {
    /* animation: loadingPulse 1.5s infinite; */
    animation: text-pulse 1s infinite;
    background-color: transparent;
    border: 1px solid #28a745;
  }

  .gamers__submit-button:disabled {
    cursor: not-allowed;
  }

  @keyframes spin {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }

  @keyframes text-pulse {
    0% {
      color: #28a745;
      transform: scale(1);
    }
    50% {
      color: #1f7a33;
      transform: scale(1.1);
    }
    100% {
      color: #28a745;
      transform: scale(1);
    }
  }

  /* Loading animation with color pulsing effect */
  @keyframes loadingPulse {
    0% {
      background-color: #28a745; /* Primary green */
    }
    50% {
      background-color: #ffcc00; /* Amber to indicate awaiting response */
    }
    100% {
      background-color: #28a745; /* Back to green */
    }
  }
</style>
