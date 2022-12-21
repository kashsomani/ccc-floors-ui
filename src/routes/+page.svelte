<script>
  import { onMount } from "svelte"
  import logo from "$lib/images/logo.png"

  const endpoint = "http://localhost:3000"
  let crocs = []

  onMount(async function () {
    const response = await fetch(endpoint)
    crocs = await response.json()
  })
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="CCC Floors" />
</svelte:head>

<img src={logo} alt="" width="200px" />
{#if crocs.length}
  {#each crocs as croc}
    {JSON.stringify(croc)}
  {/each}
{/if}
<button class="btn">hi</button>

<style>
</style>
