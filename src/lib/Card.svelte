<script>
  import { fade } from 'svelte/transition'
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()

  export let item = {}

  const { id, title, method, rating } = item
</script>

<article transition:fade>
  <h2>{title}</h2>
  <p>{method}</p>
  <span>{rating}</span>
  <button on:click={() => dispatch('update', id)}>Update</button>
  <button on:click={() => dispatch('delete')}>Delete</button>
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
