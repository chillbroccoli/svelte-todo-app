<div class="container">
  <h2 class="title">TODO</h2>
  <input class="input" type="text" bind:value on:keyup={handleKeyUp} />
  <div class="todo-list">
    {#if all}
      {#each todos as todo}
        <TodoItem {todo} {removeTodo} {toggleCompletion} />
      {/each}
    {:else if active}
      {#each todos.filter((n) => !n.completed) as todo}
        <TodoItem {todo} {removeTodo} {toggleCompletion} />
      {/each}
    {:else}
      {#each todos.filter((n) => n.completed) as todo}
        <TodoItem {todo} {removeTodo} {toggleCompletion} />
      {/each}
    {/if}
    {#if todos.length}
      <div class="buttons">
        <button class="btn btn-all" on:click={toggleAll}>All</button>
        <button class="btn btn-active" on:click={toggleActive}>Active</button>
        <button class="btn btn-completed" on:click={toggleCompleted}
          >Completed</button
        >
      </div>
    {/if}
  </div>
</div>

<script>
  import { onMount } from "svelte";
  import { v4 as uuid } from "uuid";
  import TodoItem from "./components/TodoItem.svelte";

  let value = "";
  let todos = [];
  let all = true;
  let active = false;

  onMount(() => {
    const existingTodos = localStorage.getItem("todos");
    todos = JSON.parse(existingTodos) || [];
  });

  const handleKeyUp = (event) => {
    if (event.keyCode === 13) {
      todos = [
        ...todos,
        {
          id: uuid(),
          text: value,
          completed: false,
        },
      ];

      localStorage.setItem("todos", JSON.stringify(todos));
      value = "";
    }
  };

  const removeTodo = (id) => {
    const updatedTodos = todos.filter((todo) => todo.id !== id);
    localStorage.setItem("todos", JSON.stringify(updatedTodos));
    todos = updatedTodos;
  };

  const toggleCompletion = (id) => {
    const updatedTodos = todos.map((todo) => {
      if (todo.id === id) {
        return {
          ...todo,
          completed: !todo.completed,
        };
      }

      return todo;
    });
    localStorage.setItem("todos", JSON.stringify(updatedTodos));
    todos = updatedTodos;
  };

  const toggleAll = () => {
    all = true;
    active = false;
  };

  const toggleActive = () => {
    all = false;
    active = true;
  };

  const toggleCompleted = () => {
    all = false;
    active = false;
  };
</script>

<style>
  .container {
    width: 460px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding-top: 50px;
    font-size: 2rem;
  }

  .title {
    text-transform: uppercase;
    letter-spacing: 1.2rem;
    font-size: 3.5rem;
  }

  .input {
    padding: 1rem;
    padding-left: 1.5rem;
    margin: 3rem 0;
    box-shadow: 6px 6px 12px rgba(0, 0, 0, 0.3);
    font-family: inherit;
    border: 0;
    border-bottom: 2px solid transparent;
    border-radius: 1.5rem;
    transition: border-bottom 0.5s ease-in-out;
  }

  .input:focus {
    border-bottom: 2px solid deeppink;
  }

  .todo-list {
    box-shadow: 6px 6px 12px rgba(0, 0, 0, 0.3);
    background-color: #fff;
    border-radius: 1.5rem;
  }

  .buttons {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 1.5rem;
  }

  .btn {
    border: 0;
    background: transparent;
    cursor: pointer;
    font-weight: bold;
    font-family: inherit;
  }

  .btn-all {
    color: blue;
  }

  .btn-active {
    color: black;
  }

  .btn-completed {
    color: rgba(0, 0, 0, 0.3);
  }
</style>
