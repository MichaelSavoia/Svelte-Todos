<script>
    import { tick } from 'svelte'
    import Todo from './Todo.svelte'
    let todos = JSON.parse(localStorage.getItem('todos')) || []
    let newTodo = ''
    let filterTerm = ''
    $: filteredTodos = todos.filter(todo =>
        filterTerm ? todo.title.includes(filterTerm) : true
    )

    const addNewTodo = async () => {
        if (newTodo !== '') {
            todos = [
                ...todos,
                {
                    title: newTodo,
                    completed: false,
                    id: Date.now(),
                },
            ]
            localStorage.setItem('todos', JSON.stringify(todos))
            newTodo = ''
            await tick()
            const todosList = document.getElementById('todos-list-group')
            todosList.scrollTop = todosList.scrollHeight
        }
    }

    const toggleTodoCompleted = e => {
        todos = todos.map(todo => {
            if (e.target.value == todo.id) {
                return {
                    ...todo,
                    completed: !todo.completed,
                }
            } else {
                return todo
            }
        })
        localStorage.setItem('todos', JSON.stringify(todos))
    }

    const deleteTodo = e => {
        todos = todos.filter(todo => e.target.value != todo.id)
        localStorage.setItem('todos', JSON.stringify(todos))
    }
</script>

<style>
    .wrapper {
        margin: 40px auto;
        background: #fff;
        max-width: 800px;
        height: calc(100vh - 80px);
        width: 95%;
        border-radius: 8px;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
        display: grid;
        grid-template-rows: auto 1fr auto;
        overflow: hidden;
    }

    .todos-info {
        display: flex;
        padding: 10px 15px;
        box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);
        border-bottom: 2px solid #5d58ad;
    }

    .todos-info input {
        margin: 5px 0;
    }

    .badge-group {
        flex-grow: 1;
        display: flex;
        flex-wrap: wrap;
    }

    .badge-group .badge {
        margin: 10px 0;
        margin-right: 20px;
    }

    .todos-list-group {
        overflow: auto;
    }

    .todos-list {
        margin: 0;
        padding: 8px 15px;
        list-style-type: none;
    }

    .empty-todos-list {
        margin: 0;
        padding: 20px 15px;
        font-size: 1.2rem;
        color: #b1aeb5;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
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

    .btn {
        box-shadow: 0 4px 6px rgba(50, 50, 50, 0.11), 0 1px 3px rgba(0, 0, 0, 0.08);
        transition: all 0.15s ease;
        background: #5d58ad;
        color: #fff;
        border: none;
    }

    .btn:hover {
        box-shadow: 0 7px 12px rgba(50, 50, 50, 0.1), 0 2px 4px rgba(0, 0, 0, 0.08);
        background: #7670c7;
        transform: translateY(-1px);
    }

    .btn:active {
        background: #4a458c;
        transform: translateY(1px);
    }

    @media (min-width: 576px) {
        .wrapper {
            width: 90%;
        }
    }
</style>

<div class="wrapper">
  <div class="todos-info">
  <div class="badge-group">
    <div class="badge total-todos-count">{todos.length} total todos</div>
      {#if todos.length}
        <div class="badge completed-todos-count">{todos.filter(todo => todo.completed).length} completed todos</div>
        <div class="badge todos-left-count">{todos.filter(todo => !todo.completed).length} left</div>
      {/if}
    </div>
    <div>
      <input type="text" bind:value={filterTerm} placeholder="Filter todos...">
    </div>
  </div>
  <div class="todos-list-group" id="todos-list-group">
    {#if filteredTodos.length}
      <ul class="todos-list">
        {#each filteredTodos as todo (todo.id)}
          <Todo props={{todo, deleteTodo, toggleTodoCompleted}} />
        {/each}
      </ul>
    {:else if !filteredTodos.length && filterTerm}
      <div class="empty-todos-list">No todos found.</div>
    {:else}
      <div class="empty-todos-list">Add some stuff to get done!</div>
    {/if}
  </div>
  <div class="add-todo-group">
    <form on:submit|preventDefault={addNewTodo}>
      <input type="text" bind:value={newTodo} class="add-todo-title" placeholder="Enter new todo here...">
      <button type="submit" class="add-todo btn">+ Add Todo</button>
    </form>
  </div>
</div>