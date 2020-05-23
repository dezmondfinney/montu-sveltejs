<script>
  import { onMount } from "svelte";
  import ProjectsList from './ProjectsList.svelte';
  import TaskList from './TaskList.svelte';
  import Task from "./Task.svelte";
  import TaskForm from './TaskForm.svelte';

  export let url = "";
  export let path = "tasks.json";
  let tasks = [];
  let task = {};
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
          console.log(task.project);
          
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

  // --------------
  // Functions
  // --------------
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

  const handleShowTask = (e) => {
    task = tasks.find((task) => {
      if (task.uuid === e.detail.uuid) return task
    });
  }
  const handleCloseTask = () =>  task = {} 
</script>

<main>
  <h1>Taskwarrior</h1>

  {#if typeof task.uuid !== "undefined"}
    <button on:click={handleCloseTask}>close</button>
    <TaskForm {task} />
  {/if}

  <ProjectsList {pending_projects}/>
  <TaskList tasks={pending_tasks} label="Pending Tasks" on:message={handleShowTask} />
</main>
