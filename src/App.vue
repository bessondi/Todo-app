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
        this.sortTodos()
      } else {
        fetch(`${url}?_limit=5`)
          .then(response => response.json())
          .then(data => {
            this.todosData = data
            this.sortTodos()
            this.setTodosInStorage()
            }
          )
          .catch(() => {
            console.log('Failed to get todos-data from server')
            this.todosData = [
              {
                id: 1,
                title: 'see Dmitry`s projects on GitHub',
                completed: true
              },
              {
                id: 2,
                title: 'write Dmitry a message',
                completed: false
              },
              {
                id: 3,
                title: '..or call to him',
                completed: false
              }
            ]
            this.setTodosInStorage()
          })
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
        this.setTodosInStorage()
        this.modifyTodosOnServer(url, 'POST', newTodo)
        this.sortTodos()
      },
      editTodo(todo, newTodoTitle) {
        if (todo.title !== newTodoTitle) {
          todo.title = newTodoTitle
          this.setTodosInStorage()
          this.modifyTodosOnServer(`${url}/${todo.id}`, 'PATCH', {
            title: todo.title
          })
        }
      },
      markAsDone(id) {
        let completedTodo
        this.todosData.forEach(todo => {
          if (todo.id === id) {
            todo.completed = !todo.completed
            completedTodo = todo
          }
        })

        this.sortTodos()
        this.modifyTodosOnServer(`${url}/${completedTodo.id}`, 'PATCH', {
          completed: completedTodo.completed
        })
      },
      removeTodo(id, event) {
        console.log(event)
        this.todosData = this.todosData.filter(todo => todo.id !== id)
        this.setTodosInStorage()
        this.modifyTodosOnServer(`${url}/${id}`, 'DELETE', null)
      },
      sortTodos() {
        setTimeout(() => {
          this.todosData.sort((a, b) => {
            if ( a.completed !== b.completed ) {
              return a.completed ? 1 : -1
            } else {
              return a.id > b.id ? 1 : -1;
            }
          })
          this.setTodosInStorage()
        }, 200)
      },
      setTodosInStorage(data = this.todosData) {
        localStorage.setItem('todos', JSON.stringify(data))
      },
      modifyTodosOnServer(url, method, todo) {
        fetch(url, {
          method: method,
          body: JSON.stringify(todo),
          headers: method !== 'DELETE' ? {"Content-type": "application/json; charset=UTF-8"} : undefined
        })
          .then(response => response.ok ?
          console.log(`Todo has been modified on server`) : console.log(response.statusText))
          .catch(() => console.log('Something went wrong'))
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
      background-color: $accentLightGrayColor;
      margin: 0;
    }

    #app {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      margin: 0;
      padding: 0;
      height: 100%;

      .container {
        display: flex;
        justify-content: center;
        background: $bodyBackgroundColor;
        min-height: 100vh;

        .todoList {
          display: flex;
          flex-direction: column;
          align-self: start;
          max-width: 500px;
          width: 100%;
          margin: 60px auto;
          background-color: $todoListColor;
          border-radius: 30px;
          box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
          transition: 0.3s;

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
            appearance: none;
            -webkit-appearance: none;
            background: url(data:image/svg+xml;base64,PHN2ZyBpZD0iTGF5ZXJfMSIgZGF0YS1uYW1lPSJMYXllciAxIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0Ljk1IDEwIj48ZGVmcz48c3R5bGU+LmNscy0xe2ZpbGw6I2ZmZjt9LmNscy0ye2ZpbGw6IzQ0NDt9PC9zdHlsZT48L2RlZnM+PHRpdGxlPmFycm93czwvdGl0bGU+PHJlY3QgY2xhc3M9ImNscy0xIiB3aWR0aD0iNC45NSIgaGVpZ2h0PSIxMCIvPjxwb2x5Z29uIGNsYXNzPSJjbHMtMiIgcG9pbnRzPSIxLjQxIDQuNjcgMi40OCAzLjE4IDMuNTQgNC42NyAxLjQxIDQuNjciLz48cG9seWdvbiBjbGFzcz0iY2xzLTIiIHBvaW50cz0iMy41NCA1LjMzIDIuNDggNi44MiAxLjQxIDUuMzMgMy41NCA1LjMzIi8+PC9zdmc+) no-repeat 95% 50%;
            height: 30px;
            width: 100%;
          }

          .emptyList {
            color: $accentLightGrayColor;
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
