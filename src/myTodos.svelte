<script>
  let todoItems = JSON.parse(localStorage.getItem('todos')) || [];
  let newTodo = '';

  function addTodo() {
    if (newTodo !== '') {
      todoItems = [{ id: Date.now(), text: newTodo, done: false }, ...todoItems];
      localStorage.setItem('todos', JSON.stringify(todoItems));
      newTodo = '';
    }
  }

  function removeTodo(id) {
    todoItems = todoItems.filter(item => item.id !== id);
    localStorage.setItem('todos', JSON.stringify(todoItems));
  }

  function toggleTodoDone(id) {
    todoItems = todoItems.map(item => {
      if (item.id === id) {
        item.done = !item.done;
      }
      return item;
    });
    localStorage.setItem('todos', JSON.stringify(todoItems));
  }

  function moveTodoToTop(id) {
    const index = todoItems.findIndex(item => item.id === id);
    if (index !== -1) {
      const [item] = todoItems.splice(index, 1);
      todoItems = [item, ...todoItems];
      localStorage.setItem('todos', JSON.stringify(todoItems));
    }
  }
</script>

<style>
  #todo-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
    width: 100%;
  }
  #todo-container textarea {
    width: 70vw;
    height: 18vh;
    margin-bottom: 1rem;
    padding: 0.5rem;
    font-size: 1rem;
    border: 2px solid #ccc;
    border-radius: 5px;
    resize: none;
  }
  #todo-container button {
    margin: 0.5rem;
  }
  #todo-container input[type="checkbox"] {
    margin-right: 0.5rem;
  }
</style>

<div id="todo-container">
  <textarea placeholder="Add a new todo" bind:value={newTodo} />
  <button on:click={addTodo}>Add</button>
  {#each todoItems as todoItem}
    <div>
      <input
        type="checkbox"
        checked={todoItem.done}
        on:change={() => toggleTodoDone(todoItem.id)}
      />
      <span
        style={todoItem.done ? 'text-decoration: line-through; font-size: 17pt;' : 'font-size: 17pt;'}
        on:click={() => toggleTodoDone(todoItem.id)}
      >{todoItem.text}</span>
      <button on:click={() => removeTodo(todoItem.id)}>x</button>
      <button on:click={() => moveTodoToTop(todoItem.id)}>Priority</button>
    </div>
  {:else}
    <p>No todo items yet.</p>
  {/each}
</div>
