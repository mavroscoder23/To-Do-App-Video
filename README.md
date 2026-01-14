<script lang="ts">
   import { todoStore } from '$lib/todo.store.svelte';
   import TodoItem from '../components/TodoItem.svelte';

   let inputValue = $state('');

   function handleSubmit(e: Event) {
       e.preventDefault();
       if (inputValue.trim()) {
           todoStore.add(inputValue);
           inputValue = '';
       }
   }

   $effect(() => {
       localStorage.setItem('todos', JSON.stringify(todoStore.todos));
   });
</script>

<svelte:head>
   <title>SvelteKit Todos</title>
   <meta name="description" content="A simple todo app using Svelte 5 Runes" />
</svelte:head>

<main>
   <header>
       <h1>Todo List</h1>

   </header>

   <form onsubmit={handleSubmit} class="input-group">
       <input 
           type="text" 
           bind:value={inputValue} 
           placeholder="What needs to be done?"
           aria-label="New todo"
       />
       <button type="submit" disabled={!inputValue.trim()}>Add</button>
   </form>

   <div class="todo-list">
       {#if todoStore.todos.length === 0}
           <p class="empty-state">No tasks yet. Add one above!</p>
       {:else}
           {#each todoStore.todos as todo (todo.id)}
               <TodoItem {todo} />
           {/each}
       {/if}
   </div>

   <footer>
       <p>{todoStore.todos.filter(t => !t.completed).length} items left</p>
   </footer>
</main>

<style>
   :global(body) {
       margin: 0;
       font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
       background-color: #f5f5f7;
       color: #333;
       display: flex;
       justify-content: center;
       min-height: 100vh;
       padding-top: 4rem;
   }

   main {
       width: 100%;
       max-width: 500px;
       margin: 0 1rem;
   }

   header {
       text-align: center;
       margin-bottom: 2rem;
   }

   h1 {
       font-size: 2.5rem;
       font-weight: 700;
       margin: 0;
       background: linear-gradient(135deg, #646cff 0%, #ff64d8 100%);
       -webkit-background-clip: text;
       -webkit-text-fill-color: transparent;
   }

   .subtitle {
       color: #666;
       margin-top: 0.5rem;
   }

   .input-group {
       display: flex;
       gap: 0.5rem;
       margin-bottom: 2rem;
   }

   input {
       flex: 1;
       padding: 0.8rem 1rem;
       border: 2px solid #ddd;
       border-radius: 8px;
       font-size: 1rem;
       transition: border-color 0.2s;
       outline: none;
   }

   input:focus {
       border-color: #646cff;
   }

   button {
       padding: 0 1.5rem;
       background: #646cff;
       color: white;
       border: none;
       border-radius: 8px;
       font-weight: 600;
       cursor: pointer;
       transition: opacity 0.2s;
   }

   button:disabled {
       opacity: 0.5;
       cursor: not-allowed;
   }

   button:not(:disabled):hover {
       opacity: 0.9;
   }

   .empty-state {
       text-align: center;
       color: #999;
       margin-top: 2rem;
       font-style: italic;
   }

   footer {
       margin-top: 2rem;
       text-align: center;
       color: #888;
       font-size: 0.9rem;
   }
</style>
