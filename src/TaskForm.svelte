<script>
  import { DateTime } from "luxon";
  export let task = {};
  let newAnnotation = "";

  async function handleSubmit(event) {
    console.log(newAnnotation);

    console.log(task);

    const rawResponse = await fetch("http://localhost:8080/task/update", {
      method: "POST",
      body: JSON.stringify({
        uuid: task.uuid,
        description: task.description,
        project: task.project,
        annotation: newAnnotation
      })
    });
    const content = await rawResponse.json();

    console.log(content);
    location.reload();
  }
</script>

<style>
  .taskForm {
    margin: 15px;
    padding: 15px;
    border: 1px solid var(--alert-color);
  }
  label {
    display: block;
  }
  textarea,
  input {
    display: inline-block;
    width: 100%;
  }

  .annotations ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .dates {
    display: none;
  }
</style>

<form
  action="edit/{task.id}"
  class="taskForm"
  on:submit|preventDefault={handleSubmit}>
  <label for="description">Description</label>
  <input name="description" type="text" bind:value={task.description} />

  <label for="project">Project</label>
  <input name="project" type="text" bind:value={task.project} />

  <fieldset class="dates">
    <legend>Dates</legend>
    <label for="due">Due</label>
    <input
      name="due"
      type="datetime-local"
      value={DateTime.fromISO(task.due).toISO({ includeOffset: false })} />

    <label for="scheduled">Scheduled</label>
    <input
      name="scheduled"
      type="datetime-local"
      value={DateTime.fromISO(task.scheduled).toISO({
        includeOffset: false
      })} />

  </fieldset>

  <fieldset class="annotations">
    <legend>Annotations</legend>
    {#if task.annotations}
      <ul>
        {#each task.annotations as note, i}
          <li>
            <div class="noteEntry">
              {DateTime.fromISO(note.entry).toLocaleString({
                month: 'numeric',
                day: 'numeric',
                hour: 'numeric',
                minute: '2-digit'
              })}
            </div>
            <div class="noteDescription">{note.description}</div>
          </li>
        {/each}
      </ul>
    {/if}
    <textarea name="annotation" id="annotation" bind:value={newAnnotation} />
  </fieldset>


  <input type="submit" />
</form>
