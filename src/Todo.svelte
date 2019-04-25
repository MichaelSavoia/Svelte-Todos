<script>
  export let todo
  export let toggleTodoCompleted
  export let deleteTodo
</script>

<style>
    .todo-item {
        width: 100%;
        display: flex;
        justify-content: space-between;
        padding: 12px 15px;
        border-bottom: 1px solid #dfdfdf;
    }

    .todo-item:last-child {
        border-bottom: 0;
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
        flex-grow: 1;
    }

    .todo-delete {
        color: #6b1f33;
        background: none;
        border: none;
        box-shadow: 0 4px 6px rgba(107, 31, 51, 0), 0 1px 3px rgba(107, 31, 51, 0);
        transition: all 0.15s ease;
    }

    .todo-delete:hover {
        background: #ea6b8d;
        box-shadow: 0 7px 12px rgba(234, 107, 141, 0.15),
            0 2px 4px rgba(234, 107, 141, 0.08);
        transform: translateY(-1px);
    }
</style>

<li class="todo-item"  class:completed={todo.completed}>
  <div class="todo-item-group" on:click={() => toggleTodoCompleted(todo.id)}>
    <p>{todo.title}</p>
  </div>
  <button class="todo-delete btn" on:click={() => deleteTodo(todo.id)}>
    Delete
  </button>
</li>