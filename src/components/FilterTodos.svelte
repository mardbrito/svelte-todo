<script>
  export let selectedFilter;
  export let setFilter;

  let filters = ['all', 'active', 'completed'];
</script>

<div class="filters">
  {#each filters as filter}
    <button
      on:click={() => setFilter(filter)}
      class:selected={selectedFilter === filter}
      class="filter"
    >
      {filter}
    </button>
  {/each}
</div>
