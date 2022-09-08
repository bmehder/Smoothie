<script>
  import supabase from './config/supabaseClient'
  import AddNew from './lib/AddUpdate.svelte'
  import Card from './lib/Card.svelte'

  let data = []
  let error = null
  let isShowAddUpdate = false
  let orderBy = 'created_at'

  let id = null
  let title = null
  let method = null
  let rating = null

  const getAllSmoothies = async () => {
    error = null

    const { data: smoothies, error: _error } = await supabase
      .from('smoothies')
      .select('*')
      .order(orderBy, { ascending: true })

    data = smoothies
    error = _error
  }

  const handleDelete = async evt => {
    await supabase.from('smoothies').delete().eq('id', evt.detail)
  }

  const handleClick = () => (isShowAddUpdate = !isShowAddUpdate)

  const getSmoothie = async evt => {
    const { data: smoothies, error } = await supabase
      .from('smoothies')
      .select('*')
      .eq('id', evt.detail)
      .single()

    id = smoothies.id
    title = smoothies.title
    method = smoothies.method
    rating = smoothies.rating

    isShowAddUpdate = true
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

{#if isShowAddUpdate}
  <AddNew bind:id bind:title bind:method bind:rating bind:isShowAddUpdate />
{/if}

<h1>Smoothies</h1>
<h2 on:click={handleClick}>
  <span class="material-symbols-outlined" class:isShowAddUpdate> add_circle </span>
  <p>{isShowAddUpdate ? 'Close' : 'Add New'}</p>
</h2>
<select bind:value={orderBy}>
  <option value="created_at">Created At</option>
  <option value="title">Title</option>
  <option value="rating">Rating</option>
</select>

<main use:init={data}>
  {#if error}
    <p>{error.message}</p>
  {/if}

  {#each data as item}
    <Card {item} on:delete={handleDelete} on:update={getSmoothie} />
  {/each}
</main>

<style>
  h1 {
    margin: 0;
  }
  h2 {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin: 0;
    cursor: pointer;
  }
  h2 span {
    font-size: 2rem;
    transition: all 200ms ease-in-out;
  }
  h2 span:hover {
    scale: 1.2;
  }
  .isShowAddUpdate {
    rotate: 45deg;
  }
  select {
    padding: 0.5rem 1rem;
  }
  main {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(15em, 1fr));
    gap: 2rem;
    margin-top: 2rem;
  }
</style>
