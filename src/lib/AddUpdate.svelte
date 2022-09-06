<script>
  import supabase from '../config/supabaseClient'

  export let id = null
  export let title = ''
  export let method = ''
  export let rating = ''

  const resetForm = () => (id = '') || (title = '') || (method = '') || (rating = '')

  const addSmoothie = async () =>
    await supabase.from('smoothies').insert([{ title, method, rating }])

  const updateSmoothie = async id =>
    await supabase.from('smoothies').update({ title, method, rating }).eq('id', id)

  const handleAddOrUpdate = async () => {
    if (!title || !method || !rating) return

    !id && addSmoothie()

    id && updateSmoothie(id)

    resetForm()
  }
</script>

<div>
  <h3>Add New Smoothie</h3>
  <input type="text" bind:value={title} placeholder="Title" />
  <input type="text" bind:value={method} placeholder="Method" />
  <input type="text" bind:value={rating} placeholder="Rating" />
  <button on:click={handleAddOrUpdate}>Add New / Update</button>
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
