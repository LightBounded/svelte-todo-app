<script lang="ts">
  import { flip } from "svelte/animate";
  import { fade, scale } from "svelte/transition";
  import { v4 as uuid } from "uuid";
  import Todo from "./lib/Todo.svelte";
  import type { Todo as ITodo } from "./types";

  let todos: ITodo[] = [];

  $: console.log(todos);

  let todoText = "";
  let isInvalid = false;

  const handleSubmit = () => {
    if (!todoText) {
      isInvalid = true;
      return;
    }

    todos = [
      ...todos,
      {
        id: uuid(),
        text: todoText,
        isComplete: false,
      },
    ];
    todoText = "";
    isInvalid = false;
  };

  const handleDelete = (event) => {
    const id = event.detail;
    todos = todos.filter((todo) => todo.id !== id);
  };

  const handleUpdate = (event) => {
    const { id, text } = event.detail;
    todos = todos.map((todo) => {
      if (todo.id === id) {
        return {
          ...todo,
          text,
        };
      }
      return todo;
    });
  };
</script>

<div class="flex flex-col items-center p-4">
  <form class="mb-6" on:submit|preventDefault={handleSubmit}>
    <div class="flex">
      <input
        on:input={() => isInvalid && (isInvalid = false)}
        bind:value={todoText}
        class="rounded-l border border-gray-700 bg-gray-800 px-4 py-3 outline-none transition-all duration-[250ms] {isInvalid
          ? 'border-red-500'
          : 'border-gray-700'}"
        type="text"
        placeholder="Todo name"
      />
      <button class="rounded-r border-gray-700 bg-gray-700 px-4 py-3"
        >Add</button
      >
    </div>
    {#if isInvalid}
      <small transition:fade={{ duration: 250 }} class="text-red-200"
        >Please enter the name of the todo</small
      >
    {/if}
  </form>

  <div class="flex flex-col gap-2">
    {#each todos as todo (todo.id)}
      <div animate:flip={{ duration: 500 }} in:scale out:scale>
        <Todo {todo} on:delete={handleDelete} on:update={handleUpdate} />
      </div>
    {/each}
  </div>
</div>

<style>
</style>
