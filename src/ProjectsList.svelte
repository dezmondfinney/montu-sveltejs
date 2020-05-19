<script>
export let pending_projects = []
</script>

<div class="projectList">
  <ul>
    {#each pending_projects as project, i}
      <li>{project}</li>
    {/each}
  </ul>
</div>

<style>
.projectList {
    margin: 0;
    padding: 0;
    list-style: none;
}
</style>
