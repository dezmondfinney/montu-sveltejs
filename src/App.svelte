<script>
  import { onMount } from "svelte";
  import Task from "./Task.svelte";

  export let name;
	let tasks = [];
	let task = {};

  onMount(async () => {
    const res = await fetch(`http://127.0.0.1:8080/tasks`);
		tasks = await res.json();
		task = tasks[0]
    console.log(tasks["0"].description);
  });

  const reloadTasks = () => {
    console.log("hello");
  };
</script>

<style>

</style>

<main>
	<h1>Taskwarrior</h1>
  <ul>
    {#each tasks as task, i}
      <li>
        <Task {task} />
      </li>
    {/each}
  </ul>

	<form action="submit">
		<input type="text" placeholder="Description" value={ task.description }>
		<input type="text" placeholder="Project" value={ task.project }>
		<button type="submit">Save</button>
	</form>

  <button on:click={reloadTasks}>Reload Tasks</button>
</main>
