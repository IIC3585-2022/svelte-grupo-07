<script>
	import Random from "./random.svelte";
	import Breeds from "./breeds.svelte";

	export let menu = 1;

</script>

<main>

	<ul id="menu">
		<li><a href="/" on:click|preventDefault={() => (menu = 1)}>Random </a></li> |
		<li><a href="/" on:click|preventDefault={() => (menu = 2)}> By Breeds</a></li>
	</ul>

	{#if menu === 1}
		<Random />
	{:else if menu === 2}
		<Breeds />
	{:else}
		<h1>
			Page Not Found
		</h1>
	{/if}

</main>

<style>
	ul#menu {
		list-style: none;
		margin: 0;
		padding: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 50px;
		background-color: #f5f5f5;
		border-bottom: 1px solid #ccc;
	}
	ul#menu li{
		list-style-type:none;
		justify-content: center;
	}

</style>