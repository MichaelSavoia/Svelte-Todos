<script>
  export let newTodo
  export let addNewTodo
</script>

<style>
    .add-todo-group {
        padding: 20px 15px;
        border-top: 2px solid #5d58ad;
    }

    .add-todo-group form {
        display: flex;
        flex-wrap: wrap;
    }

    .add-todo-group form input {
        flex-grow: 1;
        margin-right: 20px;
        box-shadow: inset 0 2px 4px 0 rgba(0, 0, 0, 0.06);
    }

    .add-todo {
        box-shadow: 0 4px 6px rgba(93, 88, 173, 0.15),
            0 1px 3px rgba(93, 88, 173, 0.08);
        transition: all 0.15s ease;
        background: #5d58ad;
        color: #fff;
        border: none;
    }

    .add-todo:hover {
        box-shadow: 0 7px 12px rgba(118, 112, 199, 0.15),
            0 2px 4px rgba(118, 112, 199, 0.08);
        background: #7670c7;
        transform: translateY(-1px);
    }
</style>

<div class="add-todo-group">
    <form on:submit|preventDefault={addNewTodo}>
      <input type="text" bind:value={newTodo} class="add-todo-title" placeholder="Enter new todo here...">
      <button type="submit" class="add-todo">+ Add Todo</button>
    </form>
  </div>