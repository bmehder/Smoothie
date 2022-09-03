<script>
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()

  import supabase from '../config/supabaseClient'

  export let title = ''
  export let method = ''
  export let rating = ''

  const handleClick = async () => {
    if (!title || !method || !rating) return

    const { data, error } = await supabase.from('smoothies').insert([{ title, method, rating }])

    title = ''
    method = ''
    rating = ''

    dispatch('added')
  }
</script>

<div>
  <h3>Add New Smoothie</h3>
  <input type="text" bind:value={title} placeholder="Title" />
  <input type="text" bind:value={method} placeholder="Method" />
  <input type="text" bind:value={rating} placeholder="Rating" />
  <button on:click={handleClick}>Add New</button>
</div>

<style>
  div {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    max-width: 400px;
    margin: auto;
  }
  h3 {
    margin-bottom: 0;
  }
  input,
  button {
    padding: 0.5rem;
  }
</style>
