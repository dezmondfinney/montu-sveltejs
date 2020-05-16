<script>
  import { onMount } from "svelte";
  import Task from "./Task.svelte";

  export let url = "";
  export let path = "tasks.json";
  let tasks = [];
  $: pending_tasks = tasks.filter(task => {
    if (task.status === "pending") {
      return task;
    }
  });
  $: completed_tasks = tasks.filter(task => {
    if (task.status === "completed") {
      return task;
    }
  });
  $: tags = [...new Set(tasks.map(task => task.tags))];
  $: projects = [
    ...new Set(
      tasks.map(task => {
        if (typeof task.project !== "undefined") {
          return task.project;
        }
      })
    )
  ];
  $: pending_projects = [
    ...new Set(
      pending_tasks.map(task => {
        if (typeof task.project !== "undefined") {
          return task.project;
        }
      })
    )
  ];

  onMount(async () => {
    const res = await fetch(url+path);
    tasks = await res.json();
    tasks.sort((a, b) => {
      if (a.urgency < b.urgency) return 1;
      if (a.urgency > b.urgency) return -1;
      return 0;
    });
  });

  const fetchTasks = () => {
    fetch(url+path)
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
  .taskList_pending ul,
  .taskList_completed ul {
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
      {#each pending_projects as project, i}
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

  <div class="taskList_pending">
    <h3>Pending Tasks</h3>
    <ul>
      {#each pending_tasks as task, i}
        <li>
          <Task {task} />
        </li>
      {/each}
    </ul>
  </div>

  <div class="taskList_completed">
    <h3>Completed Tasks</h3>
    <ul>
      {#each completed_tasks as task, i}
        <li>
          <Task {task} />
        </li>
      {/each}
    </ul>
  </div>
</main>
