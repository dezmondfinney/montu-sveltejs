<script>
  import { DateTime } from "luxon";
  import Icon from "svelte-awesome";
  import { faThumbsUp } from "@fortawesome/free-regular-svg-icons";
  import {
    calendar,
    exclamation,
    check,
    bell,
    tag,
    plus,
    clipboard,
    stickyNote
  } from "svelte-awesome/icons";
  export let task = {};
  const editTask = () => {
    console.log("Hello: " + task.description);
  };
</script>

<style>
  :root {
    --textColor: rgb(119, 119, 119);
  }
  /* .taskList ul {
    list-style: none;
    margin: 0 auto;
    padding: 0;
    max-width: 768px;
  } */

  .task {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-template-rows: repeat(3, auto);
    grid-template-areas:
      "d d d d D D D"
      "e e e e e . ."
      "N N N N N N n";
    border: 1px solid #aaa;
    border-radius: 4px;
    font-family: sans-serif;
    font-size: 14px;
    padding: 5px;
    margin: 10px;
    /* color: rgb(119, 119, 119); */
    color: var(--textColor);
  }

  .project span,
  .due span,
  .scheduled span,
  .description span {
    padding: 0 10px;
  }

  .description {
    align-self: start;
    display: flex;
    align-items: center;
    grid-area: d;
    font-weight: 700;
    min-height: 40px;
  }

  .dates {
    grid-area: D;
    font-size: 80%;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .details {
    grid-area: e;
    display: flex;
  }

  .project {
    display: flex;
    align-items: center;
    font-weight: 100;
    font-style: italic;
    font-size: 80%;
    color: rgb(161, 159, 159);
  }

  /* .tags {
    grid-area: t;
    display: flex;
    justify-self: start;
    align-items: center;
  } */

  .tags ul {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
  }

  .tag {
    margin: 3px 5px;
    padding: 3px 10px;
    border-radius: 15px;
    background-color: var(--textColor);
    color: #fff;
    font-size: 60%;
  }

  .addNote {
    grid-area: n;
    background-color: transparent;
    border: none;
    color: var(--textColor);
    display: none;
  }

  .notes {
    /* grid-area: N; */
    display: none;
  }

  .newNote textarea {
    background-color: white;
    margin: 0;
    margin-top: 20px;
    padding: 0;
    width: 100%;
    border: none;
    border-top: 1px solid var(--textColor);
    border-bottom: 1px solid var(--textColor);
    min-height: 150px;
    background-color: rgb(245, 245, 245);
  }

  .notes ul {
    margin-top: 20px;
  }

  .note {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(1, auto);
    grid-template-areas: "ND D D D D";
  }

  .noteDate {
    grid-area: ND;
  }

  .noteDescription {
    grid-area: D;
    border-left: 1px solid rgb(182, 182, 182);
    padding-left: 10px;
    min-height: 50px;
  }
  .noteDescription p {
    margin: 0;
  }
</style>

<div class="task">
  <div class="description">
    <i class="far fa-circle" />
    <span>{task.description}</span>
  </div>

  <div class="dates">
    <div class="scheduled">
      {#if task.scheduled}
        <i class="fas fa-calendar" />
        <span>
          {DateTime.fromISO(task.scheduled).toLocaleString({
            month: 'numeric',
            day: 'numeric',
            hour: 'numeric',
            minute: '2-digit'
          })}
        </span>
      {/if}
    </div>

    <div class="due">
      {#if task.due}
        <i class="fas fa-bell" />
        <span>
          {DateTime.fromISO(task.due).toLocaleString({
            month: 'numeric',
            day: 'numeric',
            hour: 'numeric',
            minute: '2-digit'
          })}
        </span>
      {/if}
    </div>

  </div>

  <div class="details">

    <div class="project">
      {#if task.project}
        <i class="fas fa-project-diagram" />
        <span>{task.project}</span>
      {/if}
    </div>

    <div class="tags">

      {#if task.tags}
        <ul>
          {#each task.tags as tag, i}
            <li>
              <div class="tag">{tag}</div>
            </li>
          {/each}
        </ul>
      {/if}

    </div>

    <div class="notes">
      <button class="addNote">
        <i class="fas fa-sticky-note" />
      </button>
      <div class="newNote">
        <textarea name="newNote" id="newNote" cols="" rows="" />
      </div>
      <ul>
        <li>
          <div class="note">
            <div class="noteDate">Mon, May 3, 17:30</div>
            <div class="noteDescription">
              <p>this is a note</p>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>

</div>
