<script>
    import { onMount } from 'svelte';
    export let catVotes;
    let src;

    async function getCat(){
        src = "";
        const response = await fetch('https://api.thecatapi.com/v1/images/search').then(console.log('yes'));
        const data = await response.json();
        src = await data[0].url;
        console.log(data);
    }
    
    onMount(getCat); //Aparece un gato inmediatamente al cargar la pagina

    async function voteUp(){
        catVotes++;
        getCat();
    }
</script>

<div class="cat">
    {#if src}
        <img {src} alt='random cato'>
    {:else}
        <p>Loading...</p>
    {/if}

    <button on:click={voteUp}>Cat is best</button>
    <p><strong> {catVotes} Votes</strong></p>
</div>

<style>
    div.cat {
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