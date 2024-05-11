<script>
  import { tick } from 'svelte';

  import AddTodo from './AddTodo.svelte';
  import ClearTodos from './ClearTodos.svelte';
  import FilterTodos from './FilterTodos.svelte';
  import Todo from './Todo.svelte';
  import TodosLeft from './TodosLeft.svelte';

  // state
  let todos = JSON.parse(localStorage.getItem('svelte-todos')) ?? [];

  $: {
    localStorage.setItem('svelte-todos', JSON.stringify(todos));
  }

  let selectedFilter = 'all';
  let filtering = false;

  //debug
  $: console.log(todos);

  //computed
  $: todosAmount = todos.length;
  $: filteredTodos = filterTodos(todos, selectedFilter);
  $: incompleteTodos = todos.filter((todo) => !todo.completed).length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;
  $: duration = filtering ? 0 : 250;

  //methods
  function generateRamdomId() {
    return Math.random().toString(16).slice(2);
  }

  function addTodo(todo) {
    let newTodo = {
      id: generateRamdomId(),
      text: todo,
      completed: false,
    };

    todos = [...todos, newTodo];
  }

  function toggleCompleted(event) {
    let { checked } = event.target;

    todos = todos.map((todo) => ({
      ...todo,
      completed: checked,
    }));
  }

  function completeTodo(id) {
    todos = todos.map((todo) => {
      if (todo.id === id) {
        todo.completed = !todo.completed;
      }

      return todo;
    });
  }

  function removeTodo(id) {
    todos = todos.filter((todo) => todo.id !== id);
  }

  function editTodo(id, newTodo) {
    let currentTodo = todos.findIndex((todo) => todo.id === id);
    todos[currentTodo].text = newTodo;
  }

  async function setFilter(newFilter) {
    filtering = true;
    await tick();
    selectedFilter = newFilter;
    await tick();
    filtering = false;
  }

  function filterTodos(todos, filter) {
    switch (filter) {
      case 'all':
        return todos;
      case 'active':
        return todos.filter((todo) => !todo.completed);
      case 'completed':
        return todos.filter((todo) => todo.completed);
    }
  }

  function clearCompleted() {
    todos = todos.filter((todo) => todo.completed !== true);
  }
</script>

<main>
  <h1 class="title">todos</h1>

  <section class="todos">
    <AddTodo {addTodo} {toggleCompleted} {todosAmount} />

    <ul class="todo-list">
      {#each filteredTodos as todo (todo.id)}
        <Todo {todo} {completeTodo} {removeTodo} {editTodo} {duration} />
      {/each}
    </ul>

    <div class="actions">
      <TodosLeft {incompleteTodos} />
      <FilterTodos {selectedFilter} {setFilter} />
      <ClearTodos {clearCompleted} {completedTodos} />
    </div>
  </section>
</main>
