<script>
    import { tick } from 'svelte'
    import Todo from './Todo.svelte'
    import Header from './Header.svelte'
    import Footer from './Footer.svelte'
    let todos = JSON.parse(localStorage.getItem('todos')) || []
    let newTodo = ''
    let filterTerm = ''
    $: filteredTodos = todos.filter(todo =>
        filterTerm
            ? todo.title.toLowerCase().includes(filterTerm.toLowerCase())
            : true
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

    const toggleTodoCompleted = id => {
        console.log('test')
        todos = todos.map(todo => {
            if (id == todo.id) {
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

    const deleteTodo = id => {
        todos = todos.filter(todo => id != todo.id)
        localStorage.setItem('todos', JSON.stringify(todos))
    }
</script>

<style>
    .wrapper {
        margin: 40px auto;
        max-width: 800px;
        height: calc(100vh - 80px);
        width: 95%;
        display: grid;
        grid-template-rows: auto 1fr auto;
        background: #fff;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
        border-radius: 4px;
        overflow: hidden;
    }

    .todos-list-group {
        overflow: auto;
        background: #f8fafc;
    }

    .todos-list {
        margin: 0;
        padding: 0;
        list-style-type: none;
        background: none;
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

    @media (min-width: 576px) {
        .wrapper {
            width: 90%;
        }
    }
</style>

<div class="wrapper">
  <Header bind:todos bind:filterTerm />
  <div class="todos-list-group" id="todos-list-group">
    {#if filteredTodos.length}
      <ul class="todos-list">
        {#each filteredTodos as todo (todo.id)}
          <Todo bind:todo deleteTodo={deleteTodo} toggleTodoCompleted={toggleTodoCompleted} />
        {/each}
      </ul>
    {:else if !filteredTodos.length && filterTerm}
      <div class="empty-todos-list">No todos found.</div>
    {:else}
      <div class="empty-todos-list">Add some stuff to get done!</div>
    {/if}
  </div>
  <Footer bind:newTodo addNewTodo={addNewTodo} />
</div>