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

<style>
    button {
        color: white;
        background: #ff3e00;
        padding: 20px;
        border: none;
        border-radius: 4px;
        margin-top: 5px;
    }
</style>