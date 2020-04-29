<script>
  import { onMount } from "svelte";

  import Header from "./Header.svelte";
  import Navigation from "./Navigation.svelte";
  import Grid from "./Grid.svelte";

  export let isCalendarOpen = false;
  export let date = new Date();
  export let daysInMonth = [];
  export let currentDate = new Date();
  export let availableList = [];
  export let headerTitle = "PICK YOUR DATE";

  onMount(async () => {
    setDays(date);
    availableList = [
      {
        month: "MARCH",
        day: 31,
        year: 2020
      },
      {
        month: "APRIL",
        day: 13,
        year: 2020
      }
    ];
  });

  const toggleCalendar = () => {
    isCalendarOpen = !isCalendarOpen;
  };

  const selectDate = dateInfo => {
    isCalendarOpen = !isCalendarOpen;
    headerTitle = `${dateInfo.date} ${dateInfo.month}, ${dateInfo.year}`;
  };

  const setDays = date => {
    const currentMonth = new Date(date.getFullYear(), date.getMonth());
    const previousMonth = new Date(date.getFullYear(), date.getMonth(), 0);
    const nextMonth = new Date(
      currentMonth.getFullYear(),
      currentMonth.getMonth() + 1,
      1
    );
    const firstDayPosition = new Date(
      date.getFullYear(),
      date.getMonth(),
      1
    ).getDay();
    const amountOfDaysInMonth = new Date(
      date.getMonth(),
      date.getFullYear(),
      0
    ).getDate();
    daysInMonth = [];
    for (
      let i = previousMonth.getDate() - firstDayPosition + 1;
      i <= previousMonth.getDate();
      i++
    ) {
      daysInMonth.push({
        date: i,
        background: true,
        month: previousMonth
          .toLocaleString("default", { month: "long" })
          .toUpperCase(),
        year: previousMonth.getFullYear()
      });
    }
    for (let i = 1; i <= amountOfDaysInMonth; i++) {
      if (i < 10) i = "0" + i;
      daysInMonth.push({
        date: i,
        background: false,
        month: currentMonth
          .toLocaleString("default", { month: "long" })
          .toUpperCase(),
        year: currentMonth.getFullYear()
      });
    }
    let dateIncrementer = 1;
    while (daysInMonth.length !== 42) {
      if (dateIncrementer < 10) dateIncrementer = "0" + dateIncrementer;
      daysInMonth.push({
        date: dateIncrementer,
        background: true,
        month: nextMonth
          .toLocaleString("default", { month: "long" })
          .toUpperCase(),
        year: nextMonth.getFullYear()
      });
      dateIncrementer++;
    }
  };
  const changeMonth = direction => {
    if (direction === "left") {
      date = new Date(date.setMonth(date.getMonth() - 1));
      setDays(date);
    }
    if (direction === "right") {
      date = new Date(date.setMonth(date.getMonth() + 1));
      setDays(date);
    }
  };
</script>

<style lang="scss">
  .container {
    background-color: #fff;
    position: relative;
    max-width: 35rem;
    margin: auto;
    margin-top: 5rem;
    transition: padding-bottom 1s cubic-bezier(0.4, 0, 0.17, 0.8);
  }
  .grow-me {
    padding-bottom: 26rem;
  }
</style>

<svelte:head>
  <title>Sapper Calendar</title>
</svelte:head>

<div class={`container ${isCalendarOpen ? 'grow-me' : ''}`}>
  <Navigation
    {date}
    {isCalendarOpen}
    {headerTitle}
    {toggleCalendar}
    {changeMonth} />
  {#if isCalendarOpen}
    <Grid
      {isCalendarOpen}
      {daysInMonth}
      {currentDate}
      {selectDate}
      {date}
      {availableList} />
  {/if}
  {#if isCalendarOpen}
    <Header {isCalendarOpen} />
  {/if}
</div>
