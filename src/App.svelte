<script>
  import { onMount } from "svelte";
  import Task from "./Task.svelte";

  export let url = "http://localhost:9090/tasks";
  let tasks = [];
  let task = {};

  onMount(async () => {
    const res = await fetch(url);
    tasks = await res.json();
    tasks.sort((a, b) => {
      if (a.urgency < b.urgency) return 1;
      if (a.urgency > b.urgency) return -1;
      return 0;
    });
    task = tasks[0];
  });

  const fetchTasks = () => {
    fetch(url)
      .then(response => {
        return response.json();
      })
      .then(data => {
        tasks = data;
        tasks.sort((a, b) => {
          if (a.urgency < b.urgency) return 1;
          if (a.urgency > b.urgency) return -1;
          return 0;
        });
      });
  };
</script>

<style>

</style>

<main>
	<h1>Taskwarrior</h1>
  <button on:click={fetchTasks}>Reload Tasks</button>
  <ul>
    {#each tasks as task, i}
      <li>
        <Task {task} />
      </li>
    {/each}
  </ul>
</main>