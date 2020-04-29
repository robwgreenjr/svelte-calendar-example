<script>
  import { fade } from "svelte/transition";
  export let date;
  export let headerTitle;
  export let isCalendarOpen;
  export let toggleCalendar;

  const fadeRight = (node, { delay = 0, duration = 500 }) => {
    const o = +getComputedStyle(node).opacity;
    return {
      delay,
      duration,
      css: (t, u) => {
        return `opacity: ${t * o}; transform: translateX(${u * 40}px);`;
      }
    };
  };
  const fadeLeft = (node, { delay = 0, duration = 500 }) => {
    const o = +getComputedStyle(node).opacity;
    return {
      delay,
      duration,
      css: (t, u) => {
        return `opacity: ${t * o}; transform: translateX(${u * -40}px);`;
      }
    };
  };
  const showLeft = (node, { delay = 500, duration = 500 }) => {
    return {
      delay,
      duration,
      css: (t, u) => {
        return `opacity: ${t}; transform: translateX(${u * 20}px);`;
      }
    };
  };
  const showRight = (node, { delay = 500, duration = 500 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `opacity: ${t}; transform: translateX(${u * -20}px);`
    };
  };
  const leftArrowIn = (node, { delay = 500, duration = 500 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `opacity: ${t}; transform: translateX(${u * 60}px)`
    };
  };
  const leftArrowOut = (node, { delay = 0, duration = 200 }) => {
    const o = +getComputedStyle(node).opacity;
    return {
      delay,
      duration,
      css: (t, u) => `opacity: ${t * o}; left: ${t * 2}rem`
    };
  };
  const rightArrowIn = (node, { delay = 500, duration = 500 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `opacity: ${t}; transform: translateX(${u * -60}px)`
    };
  };
  const rightArrowOut = (node, { delay = 0, duration = 200 }) => {
    const o = +getComputedStyle(node).opacity;
    return {
      delay,
      duration,
      css: (t, u) => `opacity: ${t * o}; right: ${t * 2}rem`
    };
  };
  const rightTopOpen = (node, { delay = 600, duration = 300 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * 43}deg);`
    };
  };
  const rightBottomOpen = (node, { delay = 600, duration = 300 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * -43}deg);`
    };
  };
  const leftTopOpen = (node, { delay = 600, duration = 300 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * -43}deg);`
    };
  };
  const leftBottomOpen = (node, { delay = 600, duration = 300 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * 43}deg);`
    };
  };
  const leftTopClose = (node, { delay = 0, duration = 100 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * -43}deg);`
    };
  };
  const leftBottomClose = (node, { delay = 0, duration = 100 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * 43}deg);`
    };
  };
  const rightTopClose = (node, { delay = 0, duration = 100 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * 43}deg);`
    };
  };
  const rightBottomClose = (node, { delay = 0, duration = 100 }) => {
    return {
      delay,
      duration,
      css: (t, u) => `transform: rotate(${t * -43}deg);`
    };
  };
</script>

<style lang="scss">
  .navigation {
    position: relative;
    height: 6rem;
  }
  .header-center {
    font-size: 2rem;
    text-align: center;
    position: absolute;
    left: 0;
    right: 0;
    top: 1.9rem;
    margin: auto;
    z-index: 1;
  }
  .header-left {
    font-size: 2rem;
    position: absolute;
    left: 2rem;
    top: 1.9rem;
    z-index: 1;
    transition: left 0.5s cub;
  }
  .move-right {
    left: 6rem;
  }
  .left-arrow,
  .right-arrow {
    position: absolute;
    top: 32%;
    background: transparent;
    border: none;
    width: 3rem;
    height: 2rem;
    z-index: 2;
  }
  .left-arrow {
    left: 2rem;
    .arrow-top,
    .arrow-bottom {
      display: inline-block;
      background-color: grey;
      position: absolute;
      height: 1.5px;
      left: 0.7rem;
    }
    .arrow-top {
      width: 0.8rem;
    }
    .arrow-bottom {
      width: 1rem;
    }
  }
  .left-arrow.open .arrow-top {
    transform: rotate(-43deg);
    transform-origin: 0% 0%;
  }
  .left-arrow.open .arrow-bottom {
    transform: rotate(43deg);
    transform-origin: 0% 0%;
  }
  .right-arrow {
    right: 2rem;
    .arrow-top,
    .arrow-bottom {
      display: inline-block;
      background-color: grey;
      position: absolute;
      height: 1.5px;
    }
    .arrow-top {
      width: 0.8rem;
    }
    .arrow-bottom {
      width: 1rem;
    }
  }
  .right-arrow.open .arrow-top {
    transform: rotate(43deg);
    transform-origin: 100% 100%;
  }
  .right-arrow.open .arrow-bottom {
    transform: rotate(-43deg);
    transform-origin: 100% 100%;
  }
  .open-calendar {
    background: transparent;
    border: none;
    width: 4rem;
    height: 3.7rem;
    position: absolute;
    right: 2rem;
    top: 1.5rem;
  }
  img {
    width: 100%;
  }
</style>

<div class="navigation">
  {#if isCalendarOpen}
    <button
      in:leftArrowIn
      out:leftArrowOut
      class={`left-arrow ${isCalendarOpen ? 'open' : ''}`}>
      {#if isCalendarOpen}
        <span in:leftTopOpen out:leftTopClose class="arrow-top" />
      {/if}
      {#if isCalendarOpen}
        <span in:leftBottomOpen out:leftBottomClose class="arrow-bottom" />
      {/if}
    </button>
  {/if}
  {#if !isCalendarOpen}
    <div out:fadeRight in:showLeft class="header-left">{headerTitle}</div>
  {/if}
  {#if isCalendarOpen}
    <div in:showRight out:fadeLeft class="header-center">
      {date.toLocaleString('default', { month: 'long' }).toUpperCase()}, {date.getFullYear()}
    </div>
  {/if}
  {#if isCalendarOpen}
    <button
      in:rightArrowIn
      out:rightArrowOut
      class={`right-arrow ${isCalendarOpen ? 'open' : ''}`}>
      {#if isCalendarOpen}
        <span in:rightTopOpen out:rightTopClose class="arrow-top" />
      {/if}
      {#if isCalendarOpen}
        <span in:rightBottomOpen out:rightBottomClose class="arrow-bottom" />
      {/if}
    </button>
  {/if}
  {#if !isCalendarOpen}
    <button
      on:click={toggleCalendar}
      in:fade={{ delay: 500 }}
      out:fade
      class="open-calendar">
      <img src="calendar.png" alt="Open Calendar" />
    </button>
  {/if}
</div>
