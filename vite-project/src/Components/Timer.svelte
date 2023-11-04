<script>
  import { afterUpdate } from "svelte";
    // timer variables
  let workInput = 30
  let breakInput = 15
  let timerInterval
  let timerOn = false
  let workTime = true
  let cycleCount = 0
  let timerInputVisible = true

  //reactive values
  $: workTimer = 60000 * workInput
  $: breakTimer = 60000 * breakInput
  $: displayedTimer = workTimer
  
  const handleClick = () => {
    toggleInput()
  }

  function toggleInput() {
    timerInputVisible = !timerInputVisible
  }

  function resetTimer() {
    displayedTimer = workTimer
    workTime = true
    pauseTimer()
  }

  function pauseTimer() {
    timerOn = false
    clearInterval(timerInterval)
  }

  function startTimer() {
    if (timerOn === false) {
      timerOn = true
      timerInterval = setInterval(() => (displayedTimer -= 1000), 1000)
    }
  }

  function toggleTimer() {
    if (workTime === true) {
    displayedTimer = breakTimer
    workTime = false
  } else if (workTime === false) {
    displayedTimer = workTimer
    workTime = true
    cycleCount += 1
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

<div class="timer-container">
    <div class="timer-display">
        {#if workTime === true}
          <h1 style="color: darkred;">TIME TO WORK</h1>
        {:else if workTime === false}
          <h1 style="color:goldenrod">TIME TO REST</h1>
        {/if}
      
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div style="font-size: 50px;">
          {formatTimer(displayedTimer)}
        </div>
        <div class="btn-container">
          <button style="font-size: 15px;" on:click={startTimer}>▶</button>
          <button style="font-size: 12px;" on:click={pauseTimer}>▐▐</button>
          <button style="font-size: 20px;" on:click={resetTimer}>↩</button>
        </div>
        <div style="margin-top: 20px;">
            CYCLES COMPLETED: {cycleCount}
        </div>
      </div>

    
      <div class="timer-form">
        {#if timerInputVisible}
        <div class="input-container">
          <label for="workInput">Work Duration:</label>
          <input type="numeric" id="workInput" required min="1" bind:value={workInput}>
        </div>
        <div class="input-container">
          <label for="breakInput">Break Duration:</label>
          <input type="numeric" id="breakInput" required bind:value={breakInput}>
        </div>
        {/if}
        <button on:click={handleClick} style="width: 80px; font-size:medium">Set Timer</button>
      </div>
</div>

<style>
  .timer-container {
    display: flex;
    height: 100vh;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: white;
    width: 80vw;
  }

  .timer-display {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .timer-form {
    margin-top: 10vh;
    display: flex;
    flex-direction: column;
    gap: 15px;
    width: 200px;
    justify-content: center;
    align-items: center;
  }

  .input-container {
    display: flex;
    flex-direction: column;
  }
  .btn-container {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-top: 30px;

  }

  h1 {
    font-size: 70px;
    text-align: center;
  }

  input {
    width: 100px;
  }

  button {
    background-color: #123b02;
    color: antiquewhite;
    border: 0;
    border-radius: 5px;
    width: 60px;
    height: 30px;
    font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
  }

</style>