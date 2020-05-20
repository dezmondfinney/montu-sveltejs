<script>
  import { DateTime } from "luxon";
  export let task = {};
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
</style>

<form action="edit/{task.id}" class="taskForm">
  <label for="description">Description</label>
  <input name="description" type="text" value={task.description} />

  <label for="due">Due</label>
  <input
    name="due"
    type="datetime-local"
    value={DateTime.fromISO(task.due).toISO({ includeOffset: false })} />

  <label for="scheduled">Scheduled</label>
  <input
    name="scheduled"
    type="date"
    value={DateTime.fromISO(task.scheduled).toISO({ includeOffset: false })} />

  <div class="annotations">
    <label for="annotation">Annotations</label>
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
    <textarea name="annotation" id="annotation" />
  </div>

  <label for="recur">
    <i class="fas fa-redo" />
    Recur
  </label>
  <select name="repeat" id="repeat">
    <option value="daily">daily</option>
    <option value="weekdays">weekdays</option>
    <option value="weekly">weekly</option>
    <option value="monthly">monthly</option>
    <option value="yearly">yearly</option>
  </select>

  <input type="submit" />
</form>
