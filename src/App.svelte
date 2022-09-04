<script>
  import supabase from './config/supabaseClient'
  import AddNew from './lib/AddNew.svelte'
  import Card from './lib/Card.svelte'

  let data = []
  let error = null

  let id, title, method, rating

  const getSmoothies = async () => {
    error = null
    const { data: smoothies, error: _error } = await supabase
      .from('smoothies')
      .select('*')
      .order('created_at', { ascending: true })
    data = smoothies
    error = _error
  }

  const handleDelete = async evt => {
    await supabase.from('smoothies').delete().eq('id', evt.detail)
    getSmoothies()
  }

  const handleUpdate = async evt => {
    const { data: smoothies, error } = await supabase
      .from('smoothies')
      .select('*')
      .eq('id', evt.detail)
    id = smoothies[0].id
    title = smoothies[0].title
    method = smoothies[0].method
    rating = smoothies[0].rating
  }

  getSmoothies()
</script>

<AddNew bind:id bind:title bind:method bind:rating on:added={getSmoothies} />

<h1>Smoothies</h1>
<main>
  {#if error}
    <p>{error.message}</p>
  {/if}

  {#each data as item}
    <Card {item} on:delete={handleDelete} on:update={handleUpdate} />
  {/each}
</main>

<style>
  main {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(15em, 1fr));
    gap: 2rem;
  }
</style>
