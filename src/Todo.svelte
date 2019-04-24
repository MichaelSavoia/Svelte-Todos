<script>
  export let props;
  let { todo, i, toggleTodoCompleted, deleteTodo } = props;
</script>

<style>
  .todo-item {
    width: 100%;
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
  }

  .todo-item p {
    font-size: 1.1rem;
  }

  .todo-item.completed p {
    text-decoration: line-through;
  }

  .todo-item-group {
    display: flex;
    align-items: center;
  }

  /* TODO: Create custom checkbox */
  .todo-toggle {
    opacity: 0;
  }
</style>

<li class="todo-item"  class:completed={todo.completed}>
  <div class="todo-item-group">
    <input type="checkbox" class="todo-toggle" on:click={toggleTodoCompleted} bind:checked={todo.completed} value={i} />
    <p>{todo.title}</p>
  </div>
  <button class="todo-delete btn btn-soft danger" on:click={deleteTodo} value={i}>Delete Todo</button>
</li>