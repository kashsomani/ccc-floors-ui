<script>
  import { onMount } from "svelte"
  import logo from "$lib/images/logo.png"
  const endpoint = "http://localhost:3000"
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
    SE: 200,
    "1000": 160,
    "2000": 110,
    "3000": 78,
    "4000": 56,
    "5000": 40,
    "6000": 30,
    "7000": 24,
    "8000": 22,
  }
</script>

<img src={logo} alt="" width="200px" />

{#if crocs.length}
  <div class="overflow-x-auto">
    <table class="table w-full">
      <!-- head -->
      <thead>
        <tr>
          <th>Rank</th>
          <th>Rewards</th>
          <th>Floor</th>
          <th>C4 / ADA</th>
          <th>Time last updated</th>
        </tr>
      </thead>
      {#each crocs as croc}
        <tbody>
          <!-- row 1 -->
          <tr>
            <td>{croc.rank}</td>
            <th>{croc.rewards}</th>
            <td>{croc.floor}</td>
            {#if croc.rank == max.rank}
              <th style="background-color:green;">{croc.ratio}</th>
            {:else if croc.rank == min.rank}
              <th style="background-color:red;">{croc.ratio}</th>
            {:else}
              <th>{croc.ratio}</th>
            {/if}

            <td>{getDate(croc.date)}</td>
          </tr>
        </tbody>
      {/each}
    </table>
  </div>
{/if}
<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>
