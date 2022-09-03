<script>
  import supabase from './config/supabaseClient'
  import AddNew from './lib/AddNew.svelte'
  import Card from './lib/Card.svelte'

  let data = []
  let error = null

  let title, method, rating

  const getSmoothies = async () => {
    const { data: smoothies, error: _error } = await supabase.from('smoothies').select('*')
    data = smoothies
    error = _error
  }

  getSmoothies()

  const handleDelete = async idx => {
    const { data, error } = await supabase.from('smoothies').delete().eq('id', idx)
    getSmoothies()
  }

  const handleUpdate = async idx => {
    const { data: smoothies, error } = await supabase.from('smoothies').select('*').eq('id', idx)
    title = smoothies[0].title
    method = smoothies[0].method
    rating = smoothies[0].rating
    handleDelete(smoothies[0].id)
  }
</script>

<AddNew {title} {method} {rating} on:added={getSmoothies} />

<h1>Smoothies</h1>
<main>
  {#if error}
    <p>{error.message}</p>
  {/if}

  {#each data as item, _ (item.id)}
    <Card
      {item}
      on:delete={() => handleDelete(item.id)}
      on:update={evt => handleUpdate(evt.detail)}
    />
  {/each}
</main>

<style>
  main {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(15em, 1fr));
    gap: 2rem;
  }
</style>
