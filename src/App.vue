<template>
  <div id="app">
    <div class="container">
      <div class="todoList">
        <div class="header">
          <h1 class="title">
            <span class="counter">
              {{incompleteTodosCounter.length > 0
              ? incompleteTodosCounter.length
              : null}}
            </span> todo{{incompleteTodosCounter.length > 1
            ? 's left'
            : !incompleteTodosCounter.length
            ? ' app'
            : ' left'}}
          </h1>

          <Date/>

          <AddTodo
            v-bind:id="getTodoId"
            v-on:addTask="addTodo"
          />

          <select v-model="sort" class="filter">
            <option value="all">All todos</option>
            <option value="completed">Completed</option>
            <option value="incomplete">Not Completed</option>
          </select>

          <hr/>
        </div>

        <TodoList
          v-if="sortedTodos.length"
          v-bind:tasks="sortedTodos"
          v-on:editTaskFromList="editTodo"
          v-on:markListTask="markAsDone"
          v-on:removeTaskFromList="removeTodo"
        />

        <p v-else class="emptyList"><strong>NO TASKS</strong></p>
      </div>
    </div>
  </div>
</template>

<script>
  import Date from '@/components/Date'
  import AddTodo from '@/components/AddTodo'
  import TodoList from '@/components/TodoList'

  const url = 'https://jsonplaceholder.typicode.com/todos'

  export default {
    name: 'App',

    data() {
      return {
        todosData: [],
        sort: 'all'
      }
    },

    mounted() {
      if (localStorage.todos) {
        this.todosData = JSON.parse(localStorage.todos)
      } else {
        fetch(`${url}?_limit=3`)
          .then(response => response.json())
          .then(data => {
              localStorage.setItem('todos', JSON.stringify(data))
              return this.todosData = data
            }
          )
      }
    },

    components: {
      Date,
      AddTodo,
      TodoList
    },

    computed: { // переменные
      getTodoId() {
        return this.todosData.length + 1
      },
      sortedTodos() {
        if (this.sort === 'all') {
          return this.todosData
        }

        if (this.sort === 'completed') {
          return this.todosData.filter(todo => todo.completed)
        }

        if (this.sort === 'incomplete') {
          return this.todosData.filter(todo => !todo.completed)
        }
      },
      incompleteTodosCounter() {
        if (this.todosData) {
          return this.todosData.filter(todo => !todo.completed)
        }
      }
    },

    methods: {
      addTodo(newTodo) {
        this.todosData.push(newTodo)
        this.setTodoInStorage()
        this.addTodoToServer(newTodo)
      },
      editTodo(todo, newTodoTitle) {
        if (todo.title !== newTodoTitle) {
          todo.title = newTodoTitle
          this.setTodoInStorage()
          this.editTodoFromServer(todo)
        }
      },

      markAsDone(id, todoItem) {
        let completedTodo
        this.todosData.forEach(todo => {
          if (todo.id === id) {
            todo.completed = !todo.completed
            completedTodo = todo
          }
        })

        // this.todosData.sort((a, b) => {
        //   if (a.completed !== b.completed) {
        //     return a.completed ? 1 : -1
        //   } else {
        //     return a.id > b.id ? 1 : -1;
        //   }
        // })
        //
        // const list = document.querySelector('.todoList__items').childNodes
        // const listArray = [ ...list ]
        // listArray.forEach(todo => {
        //   if (todo.childNodes[0] !== todoItem.target) {
        //     todoItem.target.checked = false
        //   }
        // })

        // console.log(todoItem.target)
        // console.log(listArray)

        this.setTodoInStorage()
        this.markTodoAsDoneOnServer(completedTodo)
      },

      removeTodo(id) {
        this.todosData = this.todosData.filter(todo => todo.id !== id)
        this.setTodoInStorage()
        this.removeTodoFromServer(id)
      },
      setTodoInStorage() {
        localStorage.setItem('todos', JSON.stringify(this.todosData))
      },
      addTodoToServer(newTodo) {
        fetch(url, {
          method: 'POST',
          body: JSON.stringify(newTodo),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
          .then(response => response.json())
          .then(json => console.log(json))
      },
      markTodoAsDoneOnServer(completedTodo) {
        fetch(`${url}/${completedTodo.id}`, {
          method: 'PATCH',
          body: JSON.stringify({
            completed: completedTodo.completed
          }),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
          .then(response => response.json())
          .then(json => console.log(json))
      },
      editTodoFromServer(editedTodo) {
        fetch(`${url}/${editedTodo.id}`, {
          method: 'PATCH',
          body: JSON.stringify({
            title: editedTodo.title
          }),
          headers: {
            "Content-type": "application/json; charset=UTF-8"
          }
        })
          .then(response => response.json())
          .then(json => console.log(json))
      },
      removeTodoFromServer(deletedTodoId) {
        fetch(`${url}/${deletedTodoId}`, {
          method: 'DELETE',
        })
          .then(response => response.ok ?
            console.log('Todo was removed from server') : console.log(response.statusText))
      }
    }
  }
</script>

<style lang="scss">
  @import 'scss/colors';


  body {
    padding: 0;
    margin: 0;
    color: $textColor;
    font-size: 1.2rem;

    hr {
      border: none;
      height: 3px;
      width: 100%;
      background-color: $accentLightBlueColor;
      margin: 0;
    }

    #app {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      margin: 0;
      padding: 0;

      .container {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 100%;

        .todoList {
          display: flex;
          flex-direction: column;
          max-width: 600px;
          min-height: 500px;
          width: 100%;
          margin: 60px auto;
          border-radius: 20px;
          box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
          transition: 0.3s;

          &:hover {
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: 0.3s;
          }

          .header {
            .title {
              font-size: 3rem;
              margin-bottom: 0;

              .counter {
                color: $accentBlueColor;
              }

              @media screen and (max-width: 480px) {
                display: flex;
                flex-direction: column;
              }
            }
          }

          .filter {
            outline: none;
            border: none;
            max-width: 125px;
            margin: 20px auto;
            color: $accentBlueColor;
            font-weight: bold;
          }

          .emptyList {
            color: $accentLightBlueColor;
          }

          @media screen and (max-width: 768px) {
            font-size: 1rem;
            display: flex;
            flex-direction: column;
            margin: 30px 20px;
          }
        }
      }
    }
  }
</style>
