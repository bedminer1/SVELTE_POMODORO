<script>
  import { afterUpdate } from "svelte";

  let workInput = 30
  let breakInput = 15
  $: displayedTimer = workTimer
  let timerInterval
  let timerOn = false
  let workTime = true

  //reactive values
  $: workTimer = 60000 * workInput
  $: breakTimer = 60000 * breakInput
  
  const handleClick = () => {
    displayedTimer = workTimer
    workTime = true
  }

  function pauseTimer() {
    timerOn = false
    clearInterval(timerInterval)
  }

  function startTimer() {
    timerOn = true
    timerInterval = setInterval(() => (displayedTimer -= 1000), 1000)
  }

  function toggleTimer() {
    if (workTime === true) {
    displayedTimer = breakTimer
    workTime = false
  } else if (workTime === false) {
    displayedTimer = workTimer
    workTime = true
    pauseTimer()
  }
  }

  const formatTimer = milliseconds => {
    const seconds = Math.floor((milliseconds / 1000) % 60);
    const minutes = Math.floor((milliseconds / 1000 / 60) % 60);
    const hours = Math.floor((milliseconds / 1000 / 60 / 60) % 24);

    return [
        hours.toString().padStart(2, "0"),
        minutes.toString().padStart(2, "0"),
        seconds.toString().padStart(2, "0")
    ].join(":");
}

  afterUpdate(() => {
    if (displayedTimer === 0 || displayedTimer < 0) {
      toggleTimer()
    }
  })

</script>

<main>
  
  {#if workTime === true}
    <p>TIME TO WORK</p>
  {:else if workTime === false}
    <p>TIME TO REST</p>
  {/if}

  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div on:click={toggleTimer}>
    {formatTimer(displayedTimer)}
  </div>
  <button on:click={startTimer}>Start</button>
  <button on:click={pauseTimer}>Pause</button>
  <button on:click={handleClick}>Reset</button>

  <div>
    <label for="workInput">Work Duration:</label>
    <input type="numeric" id="workInput" required bind:value={workInput}>
  </div>
  <div>
    <label for="breakInput">Break Duration:</label>
    <input type="numeric" id="breakInput" required bind:value={breakInput}>
  </div>
  <button on:click={handleClick}>Set Timer</button>
</main>