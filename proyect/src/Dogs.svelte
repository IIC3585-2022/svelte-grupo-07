<script>
    import { onMount } from 'svelte';
    export let dogVotes;
    let src;

    async function getDog(){
        src = "";
        const response = await fetch('https://dog.ceo/api/breeds/image/random').then(console.log('yes'));
        const data = await response.json();
        src = await data.message;
        console.log(data);
    }
    
    onMount(getDog); //Aparece un gato inmediatamente al cargar la pagina

    async function voteUp(){
        dogVotes++;
        getDog();
    }
</script>

<div class="dog">
    {#if src}
        <img {src} alt='random doggo'>
    {:else}
        <p>Loading...</p>
    {/if}

    <button on:click={voteUp}>Dog is best</button>
    <p><strong>{dogVotes} Votes</strong></p>
</div>

<style>
    div.dog {
        display: inline-grid;
    }
    img {
        height: 400px;
        width: 400px;
    }
    p {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}
    button {
        color: white;
        background: #ff3e00;
        padding: 20px;
        border: none;
        border-radius: 4px;
    }
</style>