<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import type { Todo as ITodo } from "../types";
  import DeleteIcon from "./icons/DeleteIcon.svelte";
  import EditIcon from "./icons/EditIcon.svelte";

  const dispatch = createEventDispatcher();

  export let todo: ITodo;

  let input: HTMLInputElement = null;
  let isEditing = false;
</script>

<div class="flex items-center justify-between gap-5 rounded bg-gray-800 p-5">
  <div>
    {#if isEditing}
      <input
        on:blur={() => {
          isEditing = false;

          if (!input.value) return;
          if (todo.text === input.value) return;
          
          dispatch("update", { id: todo.id, text: input.value });
        }}
        bind:this={input}
        value={todo.text}
        class="bg-gray-800 text-white outline-none"
        type="text"
        placeholder="Todo name"
      />
    {:else}
      <p class="text-gray-200">{todo.text}</p>
    {/if}
  </div>
  <div class="flex gap-2">
    <button
      class="rounded border border-red-700 bg-red-700 p-1"
      on:click={() => dispatch("delete", todo.id)}
    >
      <DeleteIcon />
    </button>
    <button
      on:click={() => {
        isEditing = true;
        setTimeout(() => input.focus(), 0);
      }}
      class="rounded border border-yellow-700 bg-yellow-700 p-1"
    >
      <EditIcon />
    </button>
  </div>
</div>
