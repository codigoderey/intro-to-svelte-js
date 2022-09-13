<script>
import { onMount } from "svelte";


	let welcome = 'Bienvenue';
  let technology = 'Javascript ES'
  let color = 'lightblue'
  $: output = welcome + ' a ' + technology
  let person = {
    firstName: 'Jhon',
    lastName: 'Doe'
  }
  let showPerson = true
  let people = [
    {id: 1, name: 'John Doe'},
    {id: 2, name: 'Mary Jane'},
    {id: 3, name: 'Juan Pablo'},
  ]
  const toggle = () => {
    color = color === 'lightblue' ? 'red' : 'lightblue'
    showPerson = !showPerson
    people = [...people, {id: 4, name: 'Jose'}]
  }
  async function getPokemon(){
    const res = await fetch('https://pokeapi.co/api/v2/pokemon/?limit=10')
    let data = res.json()
    return data
  }
  let promise = getPokemon()
  onMount(() => {
    promise = getPokemon()
  })

</script>

<main>
	<h1 style="color: {color}">Hello {output}!</h1>
  {#await promise}
    <p>Loading...</p>
  {:then result}
    {#each result.results as r (r.url)}
    <p>{r.name}</p>
    {/each}
  {:catch error}
    <p style='background-color: red'>{error.message}</p>
  {/await}
  <button on:click={toggle} type='button'>Click</button>
  <div style="background-color: {color};" class="box">
    {#if showPerson}
      <div>
        <span>{person.firstName} {person.lastName}</span>
      </div>
    {/if}
    <hr>
    {#each people as p (p.id)}
      <h4>{p.name}</h4>
    {/each}
  </div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

  .box{
    width: 400px;
    height: 400px;
    background-color: orangered;
    margin: auto;
  }
</style>