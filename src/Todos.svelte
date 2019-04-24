<script>
  import { tick } from "svelte";
  import Todo from "./Todo.svelte";
  let todos = [];
  let newTodo = "";

  const addNewTodo = async () => {
    if (newTodo !== "") {
      todos = [
        ...todos,
        {
          title: newTodo,
          completed: false
        }
      ];
      newTodo = "";
      await tick();
      const todosList = document.getElementById("todos-list");
      todosList.scrollTop = todosList.scrollHeight;
    }
  };

  const toggleTodoCompleted = e => {
    todos = todos.map((todo, index) => {
      if (e.target.value == index) {
        return {
          ...todo,
          completed: !todo.completed
        };
      } else {
        return todo;
      }
    });
  };

  const deleteTodo = e => {
    todos = todos.filter((todo, index) => e.target.value != index);
  };
</script>

<style>
  .wrapper {
    margin: 40px auto;
    background: #fff;
    max-width: 800px;
    height: 100%;
    max-height: calc(100vh - 80px);
    width: 95%;
    border-radius: 8px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
    display: grid;
    grid-template-rows: auto 1fr auto;
  }

  .todos-info {
    padding: 10px 15px;
    border-bottom: 1px solid #404041;
    display: flex;
    align-items: baseline;
    flex-wrap: wrap;
  }

  .todos-info .badge {
    margin: 10px 0;
    margin-right: 20px;
  }

  .todos-info .badge:last-child {
    margin-right: 0;
  }

  .todos-list {
    margin: 0;
    padding: 20px 15px;
    list-style-type: none;
    overflow: auto;
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
    border-top: 1px solid #404041;
  }

  .add-todo-group form {
    display: flex;
    flex-wrap: wrap;
  }

  .add-todo-group form input {
    flex-grow: 1;
    margin-right: 20px;
  }

  @media (min-width: 576px) {
    .wrapper {
      width: 90%;
    }
  }
</style>

<div class="wrapper">
  <div class="todos-info">
    <div class="badge total-todos-count">{todos.length} total todos</div>
    {#if todos.length}
      <div class="badge completed-todos-count">{todos.filter(todo => todo.completed).length} completed todos</div>
      <div class="badge todos-left-count">{todos.filter(todo => !todo.completed).length} left</div>
    {/if}
  </div>
  <ul class="todos-list" id="todos-list">
    {#if todos.length}
      {#each todos as todo, i}
        <Todo props={{todo, i, deleteTodo, toggleTodoCompleted}} />
      {/each}
    {:else}
      <p>Add some stuff to get done!</p>
    {/if}
  </ul>
  <div class="add-todo-group">
    <form on:submit|preventDefault={addNewTodo}>
      <input type="text" bind:value={newTodo} class="add-todo-title" placeholder="Enter new todo here">
      <button type="submit" class="add-todo btn">+ Add Todo</button>
    </form>
  </div>
</div>