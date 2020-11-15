<script>
  import ProgressBar from "./ProgressBar.svelte";
  import {createEventDispatcher} from "svelte";

  const totalSeconds = 3;
  let secondLeft = totalSeconds;

  let isRunning = false;

  const dispatch = createEventDispatcher();

  $: progress = ((totalSeconds-secondLeft)/totalSeconds)*100;
  const handleClick = () => {
    if (!isRunning) {
      isRunning = true;
      const timer = setInterval(() => {
        secondLeft -= 1;
        if (secondLeft == 0) {
          clearInterval(timer);
          secondLeft= totalSeconds;
          isRunning = false;
          dispatch("end");
        }
      }, 1000);
    }
  };
</script>

<style>
  h2 {
    margin: 0;
  }
  .start {
    background-color: rgb(154, 73, 73);
    width: 100%;
    margin: 10px 0;
  }
    .start[disabled] {
    background-color: rgb(194, 194, 194);
    cursor: not-allowed;
  }
</style>

<div bp="grid">
  <h2 bp="offset-5@md 4@md 12@sm">Seconds Left: {secondLeft}</h2>
</div>
<ProgressBar progress={progress}/>
<div bp="grid">
  <button
    disabled={isRunning}
    on:click={handleClick}
    bp="offset-5@md 4@md 12@sm"
    class="start">
    Start
  </button>
</div>
