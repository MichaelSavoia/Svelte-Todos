<script>
  export let todos
  export let filterTerm
</script>

<style>
    .todos-info {
        z-index: 5;
        display: flex;
        flex-wrap: wrap;
        padding: 10px 15px;
        border-bottom: 2px solid #5d58ad;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
    }

    .todos-info .input-wrapper {
        flex-grow: 1;
    }

    .todos-info input {
        width: 100%;
        margin: 5px 0;
        box-shadow: inset 0 2px 4px 0 rgba(0, 0, 0, 0.06);
    }

    .badge-group {
        flex-grow: 2;
        display: flex;
        flex-wrap: wrap;
    }

    .badge-group .badge {
        margin: 10px 0;
        margin-right: 20px;
    }

    .total-todos-count {
        background: #fff;
        color: #404041;
    }

    .todos-left-count {
        background: #ea6b8d;
        color: #6b1f33;
    }

    .completed-todos-count {
        background: #91d595;
        color: hsl(124, 65%, 30%);
    }
</style>

<div class="todos-info">
  <div class="badge-group">
    <div class="badge total-todos-count">{todos.length} total todos</div>
    {#if todos.length}
      <div class="badge completed-todos-count">{todos.filter(todo => todo.completed).length} completed todos</div>
      <div class="badge todos-left-count">{todos.filter(todo => !todo.completed).length} left</div>
    {/if}
  </div>
  <div class="input-wrapper">
    <input type="text" bind:value={filterTerm} placeholder="Filter todos...">
  </div>
</div>