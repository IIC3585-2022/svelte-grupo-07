<script>
	import { tweened } from 'svelte/motion';
  import { createEventDispatcher } from "svelte";
  let original = 20; // TYPE NUMBER OF SECONDS HERE
	let timer = tweened(original)
  let stopped = false;
  
  const dispatch = createEventDispatcher()

  const emitWinner = ()=>{
    console.log("emiting winner")
    dispatch('winner', {
        });
  }
  // ------ dont need to modify code below
  import Typewriter from "svelte-typewriter";
  setInterval(() => {
    if ($timer > 0) {
      $timer--;
    }
    else if (stopped == false){
      console.log("less than")
      emitWinner();
      stopped = true;
    }
    // if ($timer < 0) {
    //   console.log("less than")
    //   emitWinner();
    //   stopped = true;
    // }
  }, 1000);
  // setInterval(()=>{
  //   if ($timer < 0 && !stopped) {
  //     console.log("less than")
  //     emitWinner();
  //     stopped = true;
  //   }
  // }, 500)

  $: minutes = Math.floor($timer / 60);
  $: minname = minutes > 1 ? "mins" : "min";
  $: seconds = Math.floor($timer - minutes * 60)
  $: secname = seconds == 1 ? " segundo" : " segundos";

  
</script>

<main>
  <!-- <div class="flex">

    <img src="https://sveltesociety.dev/logo.svg" alt="logo" width="100" />
    <div class="title">SVELTE SOCIETY DAY</div>
  </div> -->

  {#if timer < 1}
    <!-- <Typewriter loop> -->
      <h1>Se acabó!</h1>
    <!-- </Typewriter> -->
  {:else}
<h1>Quedan  
	<span class="secs">{seconds}</span>{secname}!</h1>
<progress value={$timer/original}></progress>
  {/if}
  <!-- 	feel free to modify this text!! -->
  <!-- <ul>
    <li>
      <strong>Next talk</strong>
      : Effective Transitioning (Andrew Smith)
    </li>
    <li>
      <strong>Then</strong>
      : A Framework for the Modern Storyteller (Robert Hall)
    </li>
    <li>
      <strong>Last</strong>
      : Svelte FAQ (Rich Harris)
    </li>
  </ul> -->

  <!-- <footer>
    <p>
      <em>
        See this REPL at
        <a
          href="https://svelte.dev/repl/42ed4a09041e4e39b2f43f798eb898cb?version=3.21.0"
        >
          https://bit.ly/2zxmJiu
        </a>
      </em>
    </p>
    <p>
      <em>
        All Talks and Demos/Resources at
        <a href="https://bit.ly/3bDwMAA">https://bit.ly/3bDwMAA</a>
      </em>
    </p>
  </footer> -->
</main>

<style>
  main {
    width: 600px;
    margin: 0 auto;
  }
	
	progress {
		display: block;
		width: 100%;
	}
  
</style>
