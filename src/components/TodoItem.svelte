<div
  class="todo-item"
  transition:fly={{ x: 200 }}
  on:focus={handleMouseOver}
  on:mouseover={handleMouseOver}
  on:mouseleave={handleMouseLeave}
>
  <div class="content">
    <input
      type="checkbox"
      bind:checked={todo.completed}
      on:click={() => toggleCompletion(todo.id)}
    />
    <span class="task" class:completed={todo.completed}>{todo.text}</span>
  </div>
  {#if hovered}
    <div class="icon" on:click={() => removeTodo(todo.id)} transition:fade>
      <FaTrash />
    </div>
  {/if}
</div>

<script>
  import { fade, fly } from "svelte/transition";
  import FaTrash from "svelte-icons/fa/FaTrash.svelte";

  export let todo;
  export let removeTodo;
  export let toggleCompletion;
  let hovered = false;

  function handleMouseOver() {
    hovered = true;
  }

  function handleMouseLeave() {
    hovered = false;
  }
</script>

<style>
  .todo-item {
    padding: 1.5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }

  .task {
    margin-left: 1rem;
  }

  .completed {
    color: rgba(0, 0, 0, 0.2);
    text-decoration: line-through;
  }

  .icon {
    width: 16px;
    height: 16px;
    float: right;
    cursor: pointer;
  }
</style>
