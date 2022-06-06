<script>
    import { onMount, createEventDispatcher } from 'svelte';
    import Counter from './Counter.svelte';
    import {cat, dog} from '../../scripts/store'
    export let data;
    let src;
    let animalStore;
    let currentCount;
    const dispatch = createEventDispatcher()

    let linkToFetch = data['link']

    if (data['animal']=='cat') {
        animalStore = cat
    }
    else if (data['animal']=='dog'){
        animalStore = dog
    }

    animalStore.subscribe(value => {
        currentCount = value;
    })

    const forward = () => {
        getAnimalPicture();
        dispatch("vote", {value:event.detail, animal: data['animal']});
    };

    async function getAnimalPicture(){
        src = "";
        const response = await fetch(linkToFetch).then();
        const data = await response.json();
        try {
            // Formato para capturar la data de Gatos
            src = await data[0].url;
        } catch (e) {
            // Formato para capturar la data de Perros
            src = await data.message;
        }
    }
    
    onMount(getAnimalPicture); //Aparece un gato inmediatamente al cargar la pagina

</script>

<div class="animal">
    {#if src}
        <img {src} alt='random animal'>
    {:else}
        <p>Loading...</p>
    {/if}

    <Counter on:vote={forward}></Counter>
    <p><strong> {currentCount} Votes</strong></p>
</div>

<style>
    div.animal {
        z-index: 3;
        display: inline-grid;
    }
    img {
        height: 300px;
        width: 300px;
    }
    p {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}
</style>