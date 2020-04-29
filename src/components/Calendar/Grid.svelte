<script>
  import { fade } from "svelte/transition";

  export let isCalendarOpen;
  export let daysInMonth;
  export let currentDate;
  export let selectDate;
  export let date;
  export let availableList;

  let weeks = [6, 13, 20, 27, 34, 41];

  const checkCurrentDate = data => {
    if (
      currentDate.toLocaleString("default", { month: "long" }).toUpperCase() ===
        data.month &&
      data.date === currentDate.getDate()
    )
      return true;
  };
  const checkAvailability = date => {
    for (let i = 0; i < availableList.length; i++) {
      if (
        date.date === availableList[i].day &&
        date.month === availableList[i].month &&
        date.year === availableList[i].year
      )
        return true;
    }
  };
</script>

<style lang="scss">
  .grid {
    position: absolute;
    top: 53%;
    left: 50%;
    transform: translate(-50%, -50%);
    transition: opacity 0.3s;
    .group-row {
      display: grid;
      grid-template-columns: repeat(7, 3.5rem);
      border-top: 2px solid #dfdfdf;
      border-left: 2px solid #dfdfdf;
      span {
        text-align: center;
        height: 3.5rem;
        position: relative;
        cursor: pointer;
        border-right: 2px solid #dfdfdf;
        div {
          position: absolute;
          top: 25%;
          left: 27%;
          font-size: 1.5rem;
        }
      }
    }
    .group-row:last-child {
      border-bottom: 2px solid #dfdfdf;
    }
  }
  .show {
    opacity: 1;
  }
  .current {
    border: 2px solid teal !important;
  }
  .grey-background {
    background-color: rgba($color: #dbdbdb, $alpha: 0.3);
  }
  .green-background {
    background-color: rgba($color: #ace0a7, $alpha: 0.3);
  }
</style>

{#if isCalendarOpen}
  <div in:fade={{ delay: 1000 }} class="grid">
    {#each weeks as week}
      <div class="group-row">
        {#each daysInMonth.slice(week - 6, week + 1) as day}
          <span
            on:click={selectDate(day)}
            class={`${checkCurrentDate(day) ? 'current' : ''} ${day.background ? 'grey-background' : ''} ${checkAvailability(day) ? 'green-background' : ''}`}>
            <div data-month={day.month} data-year={day.year}>{day.date}</div>
          </span>
        {/each}
      </div>
    {/each}
  </div>
{/if}
