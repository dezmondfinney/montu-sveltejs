<script>
  import { onMount } from "svelte";
  import Task from "./Task.svelte";

  // export let url = "http://localhost:5500/tasks";
  export let url = "tasks.json";
  let task = {};
  let tasks = [];
  $: tags = [...new Set(tasks.map(task => task.tags))];
  $: projects = [...new Set(tasks.map(task => task.project))];

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
  .smartList {
    display: none;
  }
  .smartList ul,
  .projectList ul,
  .taskList ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  .tagsList {
    display: none;
  }
</style>

<main>
  <h1>Taskwarrior</h1>
  <button on:click={fetchTasks}>Reload Tasks</button>

  <div class="smartList">
    <ul>
      <li>Today</li>
      <li>Next</li>
      <li>Upcoming</li>
      <li>Completed</li>
    </ul>
  </div>

  <div class="projectList">
    <ul>
      {#each projects as project, i}
        <li>{project}</li>
      {/each}
    </ul>
  </div>

  <div class="tagsList">
    <ul>
      {#each tags as tag, i}
        <li>
          <div class="tag">{tag}</div>
        </li>
      {/each}
    </ul>
  </div>

  <div class="taskList">
    <ul>
      {#each tasks as task, i}
        <li>
          <Task {task} />
        </li>
      {/each}
    </ul>
  </div>
</main>
