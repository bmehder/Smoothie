<script>
  import { fade } from 'svelte/transition'
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()

  export let item = {}
</script>

<article transition:fade>
  <h2>{item.title}</h2>
  <p>{item.method}</p>
  <span>{item.rating}</span>
  <button on:click={() => dispatch('update', item.id)}>Update</button>
  <button on:click={() => dispatch('delete', item.id)}>Delete</button>
</article>

<style>
  article {
    position: relative;
    padding: 2rem;
    background-color: white;
    color: #242424;
    border-radius: 1rem;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  }
  span {
    position: absolute;
    top: -1.25rem;
    right: -1.25rem;
    width: 1.5rem;
    height: 1.5rem;
    padding: 0.5rem;
    text-align: center;
    font-size: 1.25rem;
    font-weight: bold;
    line-height: 1.5rem;
    border-radius: 50%;
    background-color: rebeccapurple;
    color: white;
  }
</style>
