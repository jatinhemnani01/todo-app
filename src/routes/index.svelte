<script>
  import { slide, fade } from "svelte/transition";
  import { flip } from "svelte/animate";

  let inputValue;
  let todos = [];
  function addTodo() {
    const newTodo = {
      title: inputValue,
      done: false,
      id: Math.floor(Math.random() * 11000),
    };
    todos = [newTodo, ...todos];
    inputValue = "";
  }
  function deleteTodo(id) {
    let newTodo = todos.filter((value) => value.id !== id);
    todos = newTodo;
  }
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
  />
</svelte:head>

<main>
  <form on:submit|preventDefault={addTodo}>
    <input
      type="text"
      placeholder="Enter Todo"
      class="input"
      bind:value={inputValue}
      required
    />
    <input
      style="background:rgb(73 69 232);
    color: white;
    font-size: 15px;
    padding: .3em;
    cursor:pointer;"
      type="submit"
      value="Add Todo"
    />
  </form>
  <!--  -->
  <div class="container">
    <!--  -->
    <div class="child1">
      <h3>Not Completed</h3>
      {#each todos.filter((todo) => !todo.done) as todo (todo.id)}
        <div
          class="todo-item"
          animate:flip={{ duration: 400 }}
          transition:fade={{ duration: 300 }}
          style="display: flex; flex-direction: row;"
        >
          <input type="checkbox" bind:checked={todo.done} />
          <div
            class="title"
            on:click={() => {
              todo.done = true;
            }}
          >
            {todo.title}
          </div>
          <div style="">
            <i
              style="cursor: pointer;"
              class="fa fa-trash-o"
              on:click={() => {
                deleteTodo(todo.id);
              }}
            />
          </div>
        </div>
      {/each}
    </div>
    <!--  -->
    <div class="child2">
      <h3>Completed</h3>
      {#each todos.filter((todo) => todo.done) as todo (todo.id)}
        <div
          class="todo-item2"
          animate:flip={{ duration: 400 }}
          transition:slide={{ duration: 400 }}
          style="display: flex; flex-direction: row;"
        >
          <input type="checkbox" bind:checked={todo.done} />
          <div
            class="title"
            on:click={() => {
              todo.done = false;
            }}
          >
            {todo.title}
          </div>
          <div style="">
            <i
              style="cursor:pointer;"
              on:click={() => {
                deleteTodo(todo.id);
              }}
              class="fa fa-trash-o"
            />
          </div>
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
    border-radius: 5px;
    justify-content: space-between;
  }

  .title {
    cursor: pointer;
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
    align-items: center;
  }
  form .input {
    width: 300px;
    height: 25px;
  }
  .input {
    background: #20264a;
    color: white;
    outline: none;
    border: none;
    height: 3em;
    font-size: 1.2em;
  }
</style>
