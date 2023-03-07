<script>
  let appTitle = 'Lets Create Todos'

  let editing = null

// -----------------------------------------array with todo objects
let todos = [
  { name: "Do homework", complete: false, id: 1},
  { name: "Take out the dog", complete: false, id: 2},
  { name: "Make dinner", complete: false, id: 3},
  { name: "Do dishes", complete: false, id: 4}
]

// ----------------------------------------function that will add our new todo
let name = ""

const addTodo = () => {
  let lastId = todos.length > 0 ? todos[todos.length - 1].id : 0
  let newId = lastId + 1

  let todo = {
    name: name,
    complete: false,
    id: newId
    // id: Math.random()
  }

  todos = [...todos, todo]
  name = ""

  console.log(todos)
}

// ---------------------------------------- completed task and alerts
const taskComplete = (index) => {
  console.log(index)
  todos[index].complete = !todos[index].complete
  todos[index].complete === true ? alert(`Awesome you finished task: ${todos[index].name}`) : alert(`not finished with: ${todos[index].name} ?`)
}
$: console.table(todos)

// ---------------------------------------- edits the todo
const editTodo = (id) => {
  editing = id
}

// ---------------------------------------- Deletes the todo
const deleteTodo = (id) => {
  // console.log(id)
  // checks if the id is equal to the id and it will keep it in and if not it will remove it 
  todos = todos.filter((todo) => todo.id != id) 
}

// ---------------------------------------- edits existing todos
let newName = ""
const saveTodo = (index) => {
  if (todos[index].name === '') return
  todos[index].name = newName
  newName = ""
  editing = null
}

// ---------------------------------------- Cancels editing todo
const cancelTodo = (index) => {
  editing = null
}

</script>






<main>
  <h1> {appTitle} </h1>

  <!------------------------------ adds new todo task ------------------------------>
  <div id="add-new-todo">
    <input bind:value={name} class="type-todo" placeholder="Name of task...">

    <button on:click={addTodo} class="add-todo-button">Add todo</button>
  </div>

  <!------------------------------ todo list ------------------------------>
  <ul class="todo-list">
  {#each todos as todo, index (todo.id)}

    <li class:complete={todo.complete}>

      <input on:click={() => taskComplete(index)} type="checkbox" class="check"/>

 <!------------------------------ if else for the todo item name ------------------------------>
      {#if editing === todo.id}
      <input bind:value={newName} type="text" placeholder="Edit Todo..." class="edit-todo-input"> 
      {:else}
        <div class="todo-name">{todo.name}</div>
      {/if}

      <!------------------------------ todo buttons ------------------------------>
      {#if editing === todo.id}
      <div class="todo-cancelSave">
        <button on:click={() => saveTodo(index)} class="save-todo">Save</button>
        <button on:click={() => cancelTodo(index)} class="cancel-todo">Cancel</button>
      </div>
      
      {:else}
      <div class="todo-editDelete">
        <button on:click={() => editTodo(todo.id)} class="edit-todo">Edit</button>
        <button on:click={() => deleteTodo(todo.id)} class="delete-todo">Delete</button>
      </div>
      {/if}

    </li>

 <!------------------------------ appears when todo list is empty ------------------------------>
  {:else}
  <p>You have finished all your todos &#127942;&#127942;</p>
  {/each}
  </ul>

</main>



<style>

</style>
