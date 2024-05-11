<script>
  export let addTodo;
  export let toggleCompleted;
  export let todosAmount;

  let todo = '';

  function handleSubmit() {
    addTodo(todo);
    todo = '';
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  {#if todosAmount > 0}
    <input
      on:click={toggleCompleted}
      type="checkbox"
      id="toggle-all"
      class="toggle-all"
    />
    <label aria-label="Mark all as complete" for="toggle-all">
      Mark all as complete
    </label>
  {/if}

  <!-- svelte-ignore a11y-autofocus -->
  <input
    bind:value={todo}
    id="new-todo"
    class="new-todo"
    placeholder="What needs to be done?"
    type="text"
  />
</form>
