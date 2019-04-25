<script>
  export let props
  let { todo, toggleTodoCompleted, deleteTodo } = props
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
        color: #b1aeb5;
    }

    .todo-item-group {
        display: flex;
        align-items: center;
    }

    /* TODO: Create custom checkbox */
    .todo-toggle {
        opacity: 0;
    }

    .todo-delete {
        color: #6b1f33;
        background: none;
        border: none;
        box-shadow: none;
    }

    .todo-delete:hover {
        background: #ea6b8d;
    }
</style>

<li class="todo-item"  class:completed={todo.completed}>
  <div class="todo-item-group">
    <input type="checkbox" class="todo-toggle" on:click={toggleTodoCompleted} bind:checked={todo.completed} value={todo.id} />
    <p>{todo.title}</p>
  </div>
  <button class="todo-delete btn" on:click={deleteTodo} value={todo.id}>
    Delete
  </button>
</li>