<script>
import { custom_event } from 'svelte/internal'
import { createEventDispatcher } from "svelte";

const dispatch = createEventDispatcher()

const addVote = ()=>{
    dispatch('vote', {
            value: 1
        });
}
</script>

<button on:click={addVote}>
    Votar
</button>