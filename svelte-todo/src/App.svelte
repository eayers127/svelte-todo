<script>
  let todos = [
    { id: 1, title: 'Walk the Dog', completed: false },
    { id: 2, title: 'Clean Room', completed: false },
    { id: 3, title: 'Homework', completed: false}
  ];
  let newTodo = "";
  let editTodo = null;

  function addTodo() {
    if (newTodo.trim()) {
      todos = [
        ...todos,
        { id: Date.now(), title: newTodo, completed: false },
      ];
      newTodo = "";
    }
  }

  function deleteTodo(id) {
    todos = todos.filter((todo) => todo.id !== id);
  }

  function toggleTodoCompleted(id) {
    this.$emit('toggle-completed', this.todo);
  }

  function editTodoTitle(id, title) {
    todos = todos.map((todo) => {
      if (todo.id === id) {
        return { ...todo, title };
      }
      return todo;
    });
    editTodo = null;
  }
</script>

<main>
  <h1>Svelte To Do</h1>

  <form on:submit|preventDefault={addTodo}>
    <input type="text" bind:value={newTodo} placeholder="Add a new todo" />
    <button type="submit">Add Todo</button>
  </form>

  {#if todos.length === 0}
    <p>No todos yet</p>
  {:else}
    <ul>
      {#each todos as todo (todo.id)}
        <li class:completed={todo.completed}>
			
          {#if editTodo === todo.id}
            <input type="text" value={todo.title} on:input={(e) => editTodoTitle(todo.id, e.target.value)} />
            <button on:click={() => editTodo = null}>Cancel</button>
          {:else}
            <label on:click, on:keypress={() => editTodo = todo.id}>
              <input type="checkbox" bind:checked={todo.completed} on:change={() => toggleTodoCompleted(todo.id)} />
              {todo.title}
					  </label>
					<button on:click={() => editTodo = todo.id}>Edit</button>
            <button class="deleteBtn"on:click={() => deleteTodo(todo.id)}>Delete</button>
          {/if}
        </li>
      {/each}
    </ul>
  {/if}
</main>

<style>
  li {
    display: flex;
    align-items: center;
    margin: 0.5rem 0;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 0.5rem;
  }

  li.completed {
    text-decoration: line-through;
    opacity: 0.5;
  }

  input[type="checkbox"] {
    margin-right: 0.5rem;
		padding-top: 1rem;
  }

  button {
    margin-left: auto;
  }
	button.deleteBtn{
		margin-left: 0;
	}

  input[type="text"] {
    flex: 1;
    margin-right: 0.5rem;
  }
</style>