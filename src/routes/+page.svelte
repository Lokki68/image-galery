<script lang="ts">
  import axios from "axios"
  import { onMount } from "svelte"
  import { fade, fly } from "svelte/transition"

  const apiKey = "rjBFVcPto2ujW9hs-SCmBqVd-LFrdH8nr8CDpLj7hTc"
  let query = ""

  type Result = {
    id: string
    urls: {
      regular: string
    }
    alt_description: string
  }

  let photos: Result[] = []

  const fetchData = async () => {
    const response = await axios.get(
      `https://api.unsplash.com/search/photos?page=1&query=${
        query || "nature"
      }&client_id=${apiKey}`
    )

    console.log(response.data)
    photos = response.data.results
  }

  onMount(() => {
    fetchData()
  })

  const handleOnSearch = async () => {
    if (!query) return
    await fetchData()
    query = ""
  }
</script>

<div class="container">
  <div class="header">
    <h1>Image Gallery</h1>
    <div class="input-container">
      <input type="text" class="input" bind:value={query} />
      <button class="button" on:click={handleOnSearch}>Search</button>
    </div>
  </div>
  <div class="photos">
    {#each photos as photo, i (photo.id)}
      <img
        src={photo.urls.regular}
        alt={photo.alt_description}
        class="image"
        in:fly={{ y: 200, duration: 2000, delay: i * 200 }}
        out:fade
      />
    {/each}
  </div>
</div>

<style>
  .image {
    width: 400px;
    height: 250px;
    margin: 5px;
    object-fit: cover;
  }
  .photos {
    display: flex;
    flex-wrap: wrap;
  }
  .container {
    width: 1230px;
    margin: 0 auto;
  }
  .header {
    text-align: center;
    font-size: 20px;
  }
  .input {
    padding: 10px;
    width: 400px;
    border-radius: 10px;
    outline: none;
    border: 1px solid gray;
    font-size: 20px;
  }
  .button {
    padding: 10px;
    font-size: 20px;
    background-color: cornflowerblue;
    outline: none;
    border-radius: 10px;
    border: none;
    color: white;
  }
  .input-container {
    margin-bottom: 40px;
  }
</style>
