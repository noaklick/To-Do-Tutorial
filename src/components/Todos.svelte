<!-- Todos.svelte -->

<!-- all made using this tutorial: https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Svelte_variables_props -->
<script>
    // #properties making the list of todos accessible 
    export let todos = []
    let newTodoName = ''


    // #reactive: these variables will update whenever the todos array changes
    $: totalTodos = todos.length
    $: completedTodos = todos.filter((todo) => todo.completed).length

    function removeTodo(todo) {
        todos = todos.filter((t) => t.id !== todo.id)
    }

    function addTodo() {
        // #reactive
        // written this way because reactivity is only triggered upon assignment
    todos = [...todos, { id: newTodoId, name: newTodoName, completed: false }]
    newTodoName = ''
    }

    // #reactive: generating a new id for each todo as needed
    let newTodoId
    $: {
        if (totalTodos === 0) {
        newTodoId = 1;
        } else {
        newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
        }
    }

    let filter = 'all'
    const filterTodos = (filter, todos) =>
        filter === 'active' ? todos.filter((t) => !t.completed) :
        filter === 'completed' ? todos.filter((t) => t.completed) : todos




</script>
  

<div class="todoapp stack-large">
    <!-- NewTodo -->
    <form on:submit|preventDefault={addTodo}>
        <h2 class="label-wrapper">
        <label for="todo-0" class="label__lg"> What needs to be done? </label>
      </h2>
      <!-- #properties we bind the the value of the variable to the value of the input -->
      <!-- noa note: this is super super cool. never seen a feature like this before -->
      <input bind:value={newTodoName} type="text" id="todo-0" autocomplete="off" class="input input__lg" />
        <button type="submit" disabled="" class="btn btn__primary btn__lg">
        Add
      </button>
    </form>
  
    <!-- Filter -->
    <!-- #reactive: updating the html for the filters -->
    <div class="filters btn-group stack-exception">
        <button class="btn toggle-btn" class:btn__primary={filter === 'all'} aria-pressed={filter === 'all'} on:click={() => filter = 'all'} >
          <span class="visually-hidden">Show</span>
          <span>All</span>
          <span class="visually-hidden">tasks</span>
        </button>
        <button class="btn toggle-btn" class:btn__primary={filter === 'active'} aria-pressed={filter === 'active'} on:click={() => filter = 'active'} >
          <span class="visually-hidden">Show</span>
          <span>Active</span>
          <span class="visually-hidden">tasks</span>
        </button>
        <button class="btn toggle-btn" class:btn__primary={filter === 'completed'} aria-pressed={filter === 'completed'} on:click={() => filter = 'completed'} >
          <span class="visually-hidden">Show</span>
          <span>Completed</span>
          <span class="visually-hidden">tasks</span>
        </button>
      </div>
      
  
    <!-- TodosStatus -->
    <!-- #reactive this text will update whenever a new Todo is added or a Todo is co
    marked as complete -->
    <h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>
  
    <!-- Todos -->
    <!-- #controlflow We basically use svelte to mimic loops in html -->
<!-- To-dos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
  {#each filterTodos(filter, todos) as todo (todo.id)}
    <li class="todo">
      <div class="stack-small">
        <div class="c-cb">
            <!-- #reactive the curly braces let us use JavaScript in the HTML -->
            <input type="checkbox" id="todo-{todo.id}"
            on:click={() => todo.completed = !todo.completed}
            checked={todo.completed}
          />
          
          <label for="todo-{todo.id}" class="todo-label"> {todo.name} </label>
        </div>
        <div class="btn-group">
          <button type="button" class="btn">
            Edit <span class="visually-hidden">{todo.name}</span>
          </button>
          <button type="button" class="btn btn__danger"
            on:click={() => removeTodo(todo)}>
            Delete <span class="visually-hidden">{todo.name}</span>
        </button>

        </div>
      </div>
    </li>
    {:else}
    <li>Nothing to do here!</li>
    {/each}
  </ul>
  
        
  
    <hr />
  
    <!-- MoreActions -->
    <div class="btn-group">
      <button type="button" class="btn btn__primary">Check all</button>
      <button type="button" class="btn btn__primary">Remove completed</button>
    </div>
  </div>
  