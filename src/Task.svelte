<script>
  import { DateTime } from "luxon";
  export let task = {};
</script>

<style>
  .task {
    border: 1px solid #333;
    background-color: rgba(243, 243, 243, 0.5);
    border-radius: 4px;
    margin: 15px;
    padding: 15px;
  }
  .noteList {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  .noteList > * {
    border-top: 1px solid #444;
  }
</style>

<div class="task">
  <div class="description">
    <strong>Description:</strong>
    {task.description}
  </div>

  <div class="project">
    <strong>Project:</strong>
    {#if task.project}{task.project}{:else}No Project{/if}
  </div>

  <div class="due">
    {#if task.due}
      <strong>Due:</strong>
      {DateTime.fromISO(task.due).toLocaleString({
        month: 'long',
        day: 'numeric',
        hour: 'numeric',
        minute: '2-digit'
      })}
    {/if}
  </div>

  <div class="scheduled">
    {#if task.scheduled}
      <strong>Scheduled:</strong>
      {DateTime.fromISO(task.scheduled).toLocaleString({
        month: 'long',
        day: 'numeric',
        hour: 'numeric',
        minute: '2-digit'
      })}
    {/if}
  </div>

  <div class="urgency">
    <strong>Urgency:</strong>
    {task.urgency}
  </div>

  <hr />

  <div class="tags">
    {#if task.tags}
      <strong>Tags:</strong>
      <ul>
        {#each task.tags as tag, i}
          <li>{tag}</li>
        {/each}
      </ul>
    {/if}
    <button>Add Tags</button>
  </div>

  <div class="notes">
    {#if task.annotations}
      <strong>Notes:</strong>
      <ul class="noteList">
        {#each task.annotations as note, i}
          <li>
            {DateTime.fromISO(note.entry).toLocaleString(DateTime.DATE_HUGE)}
            <br />
            <textarea name="note" id="note">{note.description}</textarea>
          </li>
        {/each}
      </ul>
    {/if}
    <button>Add a note</button>
  </div>

</div>
