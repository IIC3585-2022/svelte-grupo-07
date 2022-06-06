<script>
	import { fly, fade } from 'svelte/transition';
	import Animal from "./components/Animal.svelte";
	import Timer from "./components/Timer.svelte"
	import Confetti from './components/Confetti.svelte'
	import {cat, dog} from '../scripts/store'

	let catData = {
		'link': 'https://api.thecatapi.com/v1/images/search',
		'animal': 'cat'
	}
	let dogData = {
		'link': 'https://dog.ceo/api/breeds/image/random',
		'animal': 'dog'
	}
	let catVotes = 0;
	let dogVotes = 0;

	let toggleCat = true;
	let toggleDog = true;
	let toggleTimer = true;
	let confettiVisible = false;

	const catUnsubscribe = cat.subscribe(value => {
		catVotes = value;
	});
	const dogUnsubscribe = dog.subscribe(value => {
		dogVotes = value;
	});

	const voteReceived = (vote)=>{
		console.log("vote received for: ", vote.detail.animal )
		if (vote.detail.animal == 'cat') {
			cat.update((currentCatVotes)=>{
				return currentCatVotes + vote.detail.value
			})
		} else {
			dog.update((currentDogVotes)=>{
				return currentDogVotes + vote.detail.value
			})
		}
	}

	const announceWinner = (e) =>{
		console.log("receiving annoncement")
		if (catVotes > dogVotes) {
			toggleDog = !toggleDog
			confettiVisible = !confettiVisible;
		}
		else if (catVotes < dogVotes) {
			toggleCat = !toggleCat;
			confettiVisible = !confettiVisible;
		}
		toggleTimer= !toggleTimer
	}
</script>

<main>
	{#if confettiVisible}
	<div class='container2'>
		<Confetti durationInSeconds={6} />
	</div>
	<div class='container3'>
		<Confetti durationInSeconds={4} />
	</div>
	{/if}

	<h1>Cats v/s Dogs</h1>
	{#if toggleTimer}
	<Timer on:winner={announceWinner}></Timer>
	{/if}
	<div class="votes">
		{#if toggleCat}
		<Animal on:vote={voteReceived} data={catData}></Animal>
		{/if}
		{#if toggleDog}
		<Animal on:vote={voteReceived} data={dogData}></Animal>
		{/if}
	</div>
	{#if catVotes > dogVotes }
		{#if confettiVisible}
			<p in:fly="{{ y: 200, duration: 2000 }}" out:fade> CATS WIN!</p>
		{:else}
			<p in:fly="{{ y: 200, duration: 2000 }}" out:fade> Cats are winning</p>
		{/if}
	{:else if dogVotes > catVotes}
		{#if confettiVisible}
			<p in:fly="{{ y: 200, duration: 2000 }}" out:fade> DOGS WIN!</p>
		{:else}
			<p in:fly="{{ y: 200, duration: 2000 }}" out:fade> Dogs are winning</p>
		{/if}
	{:else}
		<p in:fly="{{ y: 200, duration: 2000 }}" out:fade> It's a tie</p>
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
	p {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}

	.container2{
		width: 00px;
		margin: 50% 80%;
	}
	.container3{
		width: 00px;
		margin: 30% 20%;
	}
</style>