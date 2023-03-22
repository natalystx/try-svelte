<script lang="ts">


type Todo = {
  label: string;
  done?: boolean;
}

  let todoInput = ''
  let todos:Todo[] = [];

  const addTodo = (e: Event) => {
    e.preventDefault();
    if(todoInput){
      todos.push({label: todoInput, done: false})
      todos = todos
      todoInput = ''
    }
  }


  const deleteTodo = (index: number) => {
    todos = todos.filter((_, i ) => i !== index)
  }


  const done = (index: number ) => {
    todos[index] = {...todos[index], done: true}
  }
  const initTodos = () => {        
      const todosInStorage = localStorage.getItem('todos')
      if(!todosInStorage){
          localStorage.setItem('todos', JSON.stringify(todos))
      }else{
        const parsed = JSON.parse(todosInStorage) as Todo[]
        todos = parsed;

      }
  }



  $: {  
    localStorage.setItem('todos', JSON.stringify(todos))        
  }

  initTodos()
</script>

<form on:submit={addTodo}>
  <div>
    <input type="text" placeholder="todo" bind:value={todoInput}>
    <button on:click={addTodo} type="submit">submit</button>
  </div>
  <div class="todos_wrapper">
  {#each todos as todo, index}
      <div class="todo">
        <input bind:value={todo.label} style:color={todo.done && 'red'} disabled={todo.done}/>
        <button on:click={() => done(index)} disabled={todo.done}>done</button>
        <button on:click={() => deleteTodo(index)}>delete</button>
      </div>
  {/each}
</div>
</form>
<div>
  <div>TODOS: </div>
  {JSON.stringify(todos)}
</div>

<style> 
.todo{
  width: 200px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 8px 0px;
}


.todos_wrapper{
  display: flex;
  flex-direction: column-reverse;
}
</style>
