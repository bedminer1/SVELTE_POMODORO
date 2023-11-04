<script>
  import { afterUpdate } from "svelte";
  import Timer from "./Components/Timer.svelte";

  //tabs variable
  let tabs = ['timerDisplay', 'timerSet']
  let currentTab = tabs[0]

</script>

<main>
  <Timer />
</main>

<style>
  main {
    display: flex;
    gap: 10vw;
    height: 100vh;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    color: white;
    width: 80vw;
  }

</style>