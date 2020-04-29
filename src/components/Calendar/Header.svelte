<script>
  import { fade } from "svelte/transition";

  export let isCalendarOpen;

  const weekDays = ["S", "M", "T", "W", "T", "F", "S"];
</script>

<style lang="scss">
  .header {
    position: absolute;
    bottom: 1.5rem;
    left: 5rem;
    span {
      font-size: 1.5rem;
      color: #ddd;
      margin: 0 1.26rem;
    }
  }
</style>

{#if isCalendarOpen}
  <div in:fade={{ delay: 1000 }} class="header">
    {#each weekDays as day}
      <span>{day}</span>
    {/each}
  </div>
{/if}
