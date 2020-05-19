<script>
export let tags = []
</script>

<div class="tagsList">
  <ul>
    {#each tags as tag, i}
      <li>
        <div class="tag">{tag}</div>
      </li>
    {/each}
  </ul>
</div>

<style>
.tagsList {
  color: red;
    margin: 0;
    padding: 0;
    list-style: none;
}
</style>