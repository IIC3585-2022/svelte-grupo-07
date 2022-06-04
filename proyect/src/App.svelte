<script>
	import Cats from "./Cats.svelte";
	import Dogs from "./Dogs.svelte";

	let catVotes = 0;
	let dogVotes = 0;
</script>

<main>
	<h1>Cats v/s Dogs</h1>

	<div class="votes">
		<Cats bind:catVotes={catVotes}/>
		<Dogs bind:dogVotes={dogVotes}/>
	</div>

	{#if catVotes > dogVotes}
		<p> Cats are winning</p>
	{:else if dogVotes > catVotes}
		<p> Dogs are winning</p>
	{:else}
		<p> It's a tie</p>
	{/if}
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

	div.votes{
		display: flexbox;
	}
</style>