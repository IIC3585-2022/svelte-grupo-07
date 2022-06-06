<script>
	import Cats from "./Cats.svelte";
	import Dogs from "./Dogs.svelte";
    import { onMount } from 'svelte';

    let dog_src;

    let cat_src;



    
    async function getAllDogBreeds(){
        const dogsrc = "https://dog.ceo/api/breeds/list/all";
        const response = await fetch(dogsrc).then(console.log('yes'));
        const data = await response.json();
        const breeds = Object.keys(data["message"])
        return(breeds);
    }

    async function getAllCatBreeds(){
        let getBreadsrc = "https://api.thecatapi.com/v1/breeds"
        const response = await fetch(getBreadsrc).then(console.log('yes'));
        const data = await response.json();
        let breeds = [];
        for (let i = 0; i < data.length; i++) {
            breeds.push({
                name: data[i].name,
                id: data[i].id
            });
        }
        return breeds;
    }

    async function getDogByBreed(breed){
        //breed = "schnauzer";
        dog_src = "";
        const link ="https://dog.ceo/api/breed/"+breed+"/images/random";
        const response = await fetch(link);
        const data = await response.json();
        dog_src = await data.message;

        }


    async function getCatByBreed(breed_id){
        //breed = "abys";
        let breedsrc = "https://api.thecatapi.com/v1/images/search?breed_ids="+breed_id;
        const response = await fetch(breedsrc);
        const data = await response.json();
        cat_src = await data[0].url;
    }

    async function voteUpDogBreed(){
        dogVotesBreed++;
        await getDogByBreed(value2);
        console.log(dog_src);
    }

    async function voteUpCatBreed(){
        catVotesBreed++;
        getCatByBreed(value1);
        console.log(cat_src);
    }

    let catBreads = getAllCatBreeds();
    
    let dogBreeds = getAllDogBreeds();

    let value1 ="abys";
    let value2 ="affenpinscher";

    

	let catVotesBreed = 0;
	let dogVotesBreed = 0;
    let catVotes = 0;
	let dogVotes = 0;
</script>

<div class="Breeds">
    
    <div class="breedsSeletion">
        Select a cat breed
        <select id="catbreed" bind:value={value1}> 
            <option value="abys">Abyssinian</option>
            {#await catBreads}
                <option>...waiting</option>
            {:then breed}
            {#each breed as cat}
                <option value={cat.id}>{cat.name}</option>
            {/each}
            {/await}
        </select> 
        Select a dog breed
        <select id="dogbreed" bind:value={value2}> 
            <option value="affenpinscher">affenpinscher</option>
            {#await dogBreeds}
                <option>...waiting</option>
            {:then breed}
            {#each breed as dog}
            <option value="{dog}">{dog}</option>
            {/each}
            {/await}
        </select> 
    </div>

	<h1>cat v/s dog</h1>
    <div class="votes">
        <div class="catBreed"> 
            {#if cat_src}
                <img {cat_src} alt='breed cato'>
            {:else}
                <p>Loading...</p>
            {/if}
            <button on:click={voteUpCatBreed}>Cat is best</button>
            <p><strong>{catVotesBreed} Votes</strong></p>
        </div>
        <div class="dogBreed">
            {#await dog_src}
                <p>Loading...</p>
            {:then dog_src} 
                <img {dog_src} alt='breed doggo'>
            {/await }
            <button on:click={voteUpDogBreed}>Dog is best</button>
            <p><strong>{dogVotesBreed} Votes</strong></p>
        </div>
    </div>

    
	{#if catVotesBreed > dogVotesBreed}
		<p> Cats breeds are winning</p>
	{:else if dogVotesBreed > catVotesBreed}
		<p> Dogs breeds are winning</p>
	{:else}
		<p> It's a tie</p>
	{/if}
</div>

<style>
	div.Breeds {
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
		div.Breeds {
			max-width: none;
		}
	}

	div.votes{
		display: flexbox;
	}

    div.breedsSeletion{
		display: flexbox;
	}

    select {
		--border: 3px solid blue;
		--borderRadius: 10px;
		--placeholderColor: blue;
	}


    div.catBreed {
        display: inline-grid;
    }
    img {
        height: 400px;
        width: 400px;
    }


    div.dogBreed {
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