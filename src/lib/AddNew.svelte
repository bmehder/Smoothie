<script>
  import supabase from '../config/supabaseClient'
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()

  export let id = null
  export let title = ''
  export let method = ''
  export let rating = ''

  const handleAdd = async () => {
    if (!title || !method || !rating) return

    id && (await supabase.from('smoothies').update({ title, method, rating }).eq('id', id))

    !id && (await supabase.from('smoothies').insert([{ title, method, rating }]))

    id = ''
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
  <button on:click={handleAdd}>Add New / Update</button>
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
