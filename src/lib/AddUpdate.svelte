<script>
  import supabase from '../config/supabaseClient'
  import { slide } from 'svelte/transition'

  export let id = null
  export let title = ''
  export let method = ''
  export let rating = ''
  export let isShowAddUpdate = false
  export let getAllSmoothies

  const resetForm = () => {
    id = ''
    title = ''
    method = ''
    rating = ''
  }

  const addSmoothie = async () =>
    await supabase.from('smoothies').insert([{ title, method, rating }])

  const updateSmoothie = async id =>
    await supabase.from('smoothies').update({ title, method, rating }).eq('id', id)

  const handleAddOrUpdate = async () => {
    if (!title || !method || !rating) return

    !id && addSmoothie()

    id && updateSmoothie(id)

    resetForm()

    isShowAddUpdate = false

    getAllSmoothies()
  }
</script>

<form on:submit|preventDefault={handleAddOrUpdate} transition:slide>
  <h3>Add New / Update</h3>
  <input type="text" bind:value={title} placeholder="Title" />
  <input type="text" bind:value={method} placeholder="Method" />
  <input type="text" bind:value={rating} placeholder="Rating" />
  <input type="submit" value="Add New / Update" />
</form>

<style>
  form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    max-width: 400px;
    margin: auto;
  }
  h3 {
    margin-bottom: 0;
    text-align: center;
  }
  input {
    padding: 0.5rem;
  }
</style>
