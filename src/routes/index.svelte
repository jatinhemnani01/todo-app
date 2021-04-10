<script>
  import { slide, fade } from "svelte/transition";
  import { flip } from "svelte/animate";

  let inputValue;
  let todos = [];
  function addTodo() {
    const newTodo = {
      title: inputValue,
      done: false,
      id: todos.length,
    };
    todos = [newTodo, ...todos];
    inputValue = "";
  }
</script>

<main>
  <form on:submit|preventDefault={addTodo}>
    <input
      type="text"
      placeholder="Enter Todo"
      class="input"
      bind:value={inputValue}
      required
    />
    <input type="submit" value="Add Todo" />
  </form>
  <!--  -->
  <div class="container">
    <!--  -->
    <div class="child1">
      <h3>Not Completed</h3>
      {#each todos.filter((todo) => !todo.done) as todo (todo.id)}
        <div
          on:click={() => {
            todo.done = true;
          }}
          class="todo-item"
          animate:flip={{ duration: 400 }}
          transition:fade={{ duration: 300 }}
          style="display: flex; flex-direction: row;"
        >
          <input type="checkbox" bind:checked={todo.done} />
          <div>{todo.title}</div>
        </div>
      {/each}
    </div>
    <!--  -->
    <div class="child2">
      <h3>Completed</h3>
      {#each todos.filter((todo) => todo.done) as todo (todo.id)}
        <div
          on:click={() => {
            todo.done = false;
          }}
          class="todo-item2"
          animate:flip={{ duration: 400 }}
          transition:slide={{ duration: 400 }}
          style="display: flex; flex-direction: row;"
        >
          <input type="checkbox" bind:checked={todo.done} />
          <div>{todo.title}</div>
        </div>
      {/each}
    </div>
    <!--  -->
  </div>
</main>

<style>
  .todo-item,
  .todo-item2 {
    display: flex;
    margin: 10px;
    align-items: center;
    cursor: pointer;
    border-radius: 5px;
  }

  .todo-item {
    background-color: #e84545;
    padding: 10px;
    font-size: 1.1em;
  }

  .todo-item2 {
    background-color: #9c3d54;
    padding: 10px;
    font-size: 1.1em;
    text-decoration: line-through;
  }

  .container {
    margin: 1em;
    height: 90vh;
    display: flex;
    text-align: center;
    justify-content: center;
  }
  .child1,
  .child2 {
    flex: 1;
    background-color: #2b2e4a;
    color: white;
    padding: 1em;
  }
  form {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  form .input {
    width: 300px;
    height: 25px;
  }
</style>
