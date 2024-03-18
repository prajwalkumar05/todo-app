<template>
  <div id="app">
    <Navbar />
    <TodoCounter :todos-list="todos" />
    <TodoInput @input-value="handleInputValue" />
    <p v-if="todos.length === 0" class="todos_list_alert_text">Add some todos!</p>
    <TodoList v-for="(todo, index) in todos" :todo="todo" :key="index" @remove-todo="deleteTodo" :index="index" />
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import TodoList from './components/TodoList.vue'
import TodoCounter from './components/TodoCounter.vue'
import TodoInput from './components/TodoInput.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    TodoCounter,
    TodoList,
    TodoInput,
  },
  data() {
    return {
      todos: [],
    }
  },
  methods: {
    handleInputValue(inputValue) {
      this.todos.push({
        taskName: inputValue,
        isDone: false,
        isEditing: false
      })
      console.log(this.todos)
    },

    deleteTodo(id) {
      console.log(id)
      this.todos = this.todos.filter((todo, i) => i !== id);
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-size: 10px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 60px 0;
  background-color: #0d0d0d;
  min-height: 100vh;
}

.todos_list_alert_text{
  font-size: 2rem;
  color: #CEBEA4;
}


@media only screen and (max-width: 500px) {
  html {
    font-size: 8px;
  }
}
</style>
