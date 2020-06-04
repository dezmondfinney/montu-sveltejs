<script>
  import { tasksStore, taskStore } from "./stores/TaskStore.js";
  import { onMount } from "svelte";
  import ProjectsList from "./ProjectsList.svelte";
  import TaskList from "./TaskList.svelte";
  import Task from "./Task.svelte";
  import TaskForm from "./TaskForm.svelte";

  export let url = "http://localhost:8080/";
  export let path = "tasks.json";

  $: tasks = $tasksStore;
  $: task = $taskStore;

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
    fetchTasks();
  });

  // --------------
  // Functions
  // --------------
  const fetchTasks = () => {
    fetch(url + path)
      .then(response => {
        return response.json();
      })
      .then(data => {
        let t = data;
        t.sort((a, b) => {
          if (a.urgency < b.urgency) return 1;
          if (a.urgency > b.urgency) return -1;
          return 0;
        });
        tasksStore.set(t);
        
      });
  };

  const handleShowTask = e => {
    task = tasks.find(task => {
      if (task.uuid === e.detail.uuid) return task;
    });
  };
  const handleCloseTask = () => (task = {});
</script>

<main>
  <h1>Taskwarrior</h1>
  <button on:click={fetchTasks}>Reload Tasks</button>

  {#if typeof task.uuid !== 'undefined'}
    <button on:click={handleCloseTask}>close</button>
    <TaskForm {task} />
  {/if}

  <ProjectsList {pending_projects} />
  <TaskList
    tasks={pending_tasks}
    label="Pending Tasks"
    on:message={handleShowTask} />
</main>
