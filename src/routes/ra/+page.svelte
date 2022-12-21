<script>
  import { onMount } from "svelte"
  import { fade } from "svelte/transition"
  const endpoint = "https://ccc-floor-by-rank-express.vercel.app/ra"
  let crocs = []
  const max = {
    rank: "",
    ratio: 0,
  }
  const min = {
    rank: "",
    ratio: 1000000,
  }
  onMount(async function () {
    const response = await fetch(endpoint)
    crocs = await response.json()
    crocs = crocs.reverse().map(el => ({
      ...el,
      rewards: rewards[el.rank],
      ratio: (rewards[el.rank] / el.floor).toFixed(4),
    }))
    crocs.forEach(el => {
      if (el.ratio > max.ratio) {
        max.rank = el.rank
        max.ratio = el.ratio
      }
      if (el.ratio < min.ratio) {
        min.rank = el.rank
        min.ratio = el.ratio
      }
    })
  })
  const getDate = isoString => {
    // const date = new Date(isoString).toLocaleTimeString()
    return new Date(isoString).toLocaleTimeString()
    // const timestampWithOffset =
    // return date.getTime().toLocaleTimeString()
    // const offset = date.getTimezoneOffset() * 60 * 1000
    // const timestampWithoutOffset = timestampWithOffset - offset
    // const dateWithOffset = new Date(timestampWithOffset)
    // const dateWithoutOffset = new Date(timestampWithoutOffset)
    // return dateWithoutOffset
  }
  const rewards = {
    "500": 100,
    "1000": 80,
    "2000": 55,
    "3000": 39,
    "4000": 28,
    "5000": 20,
    "6000": 15,
    "7000": 12,
    "8000": 11,
    "9000": 10,
    "10000": 10,
  }
</script>

<div class="hero min-h-16 bg-base-100">
  <div class="hero-content text-center">
    <div class="max-w-md">
      <h1 class="text-5xl font-bold text-croc">RA COLLECTION</h1>
    </div>
  </div>
</div>
{#if crocs.length}
  <div class="" in:fade={{ duration: 2000 }}>
    <table class="table w-full">
      <!-- head -->
      <thead>
        <tr class="heading">
          <th>Max Rank</th>
          <th>Rewards</th>
          <th>Floor</th>
          <th>C4 / ADA </th>
          <th>Updated at</th>
        </tr>
      </thead>
      <tr><td class="empty" colspan="5" /></tr>
      {#each crocs as croc}
        <tbody>
          <!-- row 1 -->
          <tr>
            <td>{croc.rank}</td>
            <td>{croc.rewards}</td>
            <td>{croc.floor}</td>
            {#if croc.rank == max.rank}
              <td class="best">{croc.ratio}</td>
            {:else if croc.rank == min.rank}
              <td class="worst">{croc.ratio}</td>
            {:else}
              <td>{croc.ratio}</td>
            {/if}

            <td class="date">{getDate(croc.date)}</td>
          </tr>
        </tbody>
      {/each}
    </table>
  </div>
{/if}

<svelte:head>
  <title>CCC Floors</title>
  <meta name="description" content="CCC Floors" />
</svelte:head>

<style lang="postcss">
  thead {
    border-bottom: 2px solid white;
    @apply text-2xl text-primary-content;
  }
  table {
    table-layout: fixed;
    width: 100%;
    border-spacing: 4px;
    border-collapse: separate;
    @apply bg-base-100 m-1 sm:m-3 sm:p-10 sm:mx-2 sm:pt-0;
  }
  td {
    text-align: center;
    border-radius: 10px;
    background-color: rgb(42, 51, 45);
    font-size: x-small;
    @apply sm:text-base m-1 sm:m-3;
  }
  .date {
    font-size: 0.45rem;
    @apply sm:text-base m-1 sm:m-3;
  }
  th {
    text-align: center;
    border-radius: 10px;
    font-size: xx-small;
    @apply m-1 sm:m-3 bg-base-100 sm:text-sm;
  }
  .best {
    @apply bg-primary;
  }
  .worst {
    @apply bg-error text-black;
  }
  .empty {
    border-top: 2px solid rgb(42, 51, 45);
    border-radius: 0;
    @apply bg-base-100;
  }
</style>
