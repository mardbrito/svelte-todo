<script>
  import { fade, slide } from 'svelte/transition';

  export let todo;
  export let completeTodo;
  export let removeTodo;
  export let editTodo;
  export let duration;

  let editing = false;

  function toggleEdit() {
    editing = true;
  }

  function handleEdit(event, id) {
    let presseKey = event.key;
    let targetElement = event.target;
    let newTodo = targetElement.value;

    switch (presseKey) {
      case 'Escape':
        targetElement.blur();
        break;
      case 'Enter':
        editTodo(id, newTodo);
        targetElement.blur();
        break;
    }
  }

  function handleBlur(event, id) {
    let targetElement = event.target;
    let newTodo = targetElement.value;

    editTodo(id, newTodo);
    targetElement.blur();
    editing = false;
  }
</script>

<li in:slide={{ duration }} out:fade={{ duration }} class:editing class="todo">
  <div class="todo-item">
    <input
      on:change={() => completeTodo(todo.id)}
      checked={todo.completed}
      id="todo"
      class="toggle"
      type="checkbox"
    />
    <label aria-label="Check todo" for="todo" class="todo-check" />

    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <span
      on:dblclick={toggleEdit}
      class:completed={todo.completed}
      class="todo-text"
    >
      {todo.text}
    </span>
    <button
      class="remove"
      on:click={() => removeTodo(todo.id)}
      aria-label="Remove todo">x</button
    >
  </div>

  {#if editing}
    <!-- svelte-ignore a11y-autofocus -->
    <input
      on:keydown={(event) => handleEdit(event, todo.id)}
      on:blur={(event) => handleBlur(event, todo.id)}
      type="text"
      class="edit"
      value={todo.text}
      autofocus
    />
  {/if}
</li>

<style>
  .completed {
    text-decoration: line-through;
  }
</style>
