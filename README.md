<script lang="ts">
    import type { Todo } from '$lib/types';
    import { todoStore } from '$lib/todo.store.svelte';

    let { todo } = $props<{ todo: Todo }>();
</script>

<div class="todo-item" class:completed={todo.completed}>
    <input 
        type="checkbox" 
        checked={todo.completed} 
        onchange={() => todoStore.toggle(todo.id)}
    />
    <span class="text">{todo.text}</span>
    <button onclick={() => todoStore.delete(todo.id)} aria-label="Delete">
        ✕
    </button>
</div>

<style>
    .todo-item {
        display: flex;
        align-items: center;
        gap: 1rem;
        padding: 0.75rem;
        background: white;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        transition: transform 0.2s, opacity 0.2s;
        margin-bottom: 0.5rem;
    }

    .todo-item:hover {
        transform: translateY(-1px);
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }

    .completed .text {
        text-decoration: line-through;
        color: #888;
    }

    .text {
        flex: 1;
        font-size: 1rem;
        color: #333;
    }

    input[type="checkbox"] {
        width: 1.2rem;
        height: 1.2rem;
        cursor: pointer;
        accent-color: #646cff;
    }

    button {
        background: none;
        border: none;
        color: #ff4444;
        font-size: 1.2rem;
        cursor: pointer;
        padding: 0.25rem 0.5rem;
        border-radius: 4px;
        transition: background 0.2s;
    }

    button:hover {
        background: #fff0f0;
    }
</style>
