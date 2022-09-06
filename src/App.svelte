<script>
  import supabase from './config/supabaseClient'
  import AddNew from './lib/AddUpdate.svelte'
  import Card from './lib/Card.svelte'

  let data = []
  let error = null

  const form = {
    id: null,
    title: null,
    method: null,
    rating: null,
  }

  const getAllSmoothies = async () => {
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
  }

  const getSmoothie = async evt => {
    const { data: smoothies, error } = await supabase
      .from('smoothies')
      .select('*')
      .eq('id', evt.detail)
      .single()

    form.id = smoothies.id
    form.title = smoothies.title
    form.method = smoothies.method
    form.rating = smoothies.rating
  }

  const init = (node, params) => {
    getAllSmoothies()

    return {
      update(newParams) {
        getAllSmoothies()
      },
    }
  }
</script>

<AddNew {...form} />

<h1>Smoothies</h1>
<main use:init={data}>
  {#if error}
    <p>{error.message}</p>
  {/if}

  {#each data as item}
    <Card {item} on:delete={handleDelete} on:update={getSmoothie} />
  {/each}
</main>

<style>
  main {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(15em, 1fr));
    gap: 2rem;
  }
</style>
