<script>
  import Task from "./Task.svelte";
  export let tasks = [];
  export let label = "Tasks";
</script>

<div class="taskList">
    <h3>{label}</h3>
    <ul>
      {#each tasks as task, i}
        <li>
          <Task {task} />
        </li>
      {/each}
    </ul>
</div>

<style>
  .taskList ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
</style>