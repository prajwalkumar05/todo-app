<template>
  <div id="app">
    <Navbar />
    <TodoCounter :todos-list="todos" />
    <PriorityIndicator />
    <TodoInput :initial-value="todoToEdit" @input-value="handleInputValue" />

    <p v-if="todos.length === 0" class="todos_list_alert_text">Add some todos!</p>

    <TodoList v-for="(todo, index) in sortedTasks" :todo="todo" :key="index" @remove-todo="deleteTodo" :index="index"
      @edit-todo="editTodo" @show-toast="showToast" @swapthe-value="swapThevalue" />


    <MessageAlert :message="toastMessage" ref="toast" />

  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import TodoList from './components/TodoList.vue'
import TodoCounter from './components/TodoCounter.vue'
import TodoInput from './components/TodoInput.vue'
import MessageAlert from './components/MessageAlert.vue'
import PriorityIndicator from './components/PriorityIndicator.vue'

export default {
  name: 'App',
  components: {
    Navbar,
    TodoCounter,
    TodoList,
    TodoInput,
    MessageAlert,
    PriorityIndicator
  },
  data() {
    return {
      todos: [],
      toastMessage: "",
      counter: 1,
      todoToEdit: "",
      editingTask: null,
      swapValue1: null,
      swapValue2: null,
    }
  },
  computed: {
    sortedTasks() {
      const priorityOrder = { high: 0, medium: 1, low: 2 };
      const sorted = this.todos.slice().sort((a, b) => priorityOrder[a.selectedPriority] - priorityOrder[b.selectedPriority]);
      console.log("sorted");
      console.log(sorted);
      return sorted;
    }
  },

  methods: {
    handleInputValue(inputValue, selectedPriority) {
      console.log(inputValue, selectedPriority)

      if (inputValue === "") {
        return
      }

      if (this.editingTask != null) {
        const todoToUpdate = this.todos.find(todo => todo.id === this.editingTask);
        if (todoToUpdate) {
          todoToUpdate.taskName = inputValue;
        }
        this.editingTask = null;
      }
      else {
        this.todos.push({
          id: this.counter++, taskName: inputValue, isDone: false, isEditing: false, selectedPriority: selectedPriority, isSwap: false
        })
        this.showToast("Todo Adedd")
        console.log(this.todos)
      }
    },

    deleteTodo(id) {
      console.log(id)
      this.todos = this.todos.filter((todo) => todo.id !== id);
      this.editingTask = null;
    },

    showToast(message) {
      this.toastMessage = message;
      this.$refs.toast.showToast();
    },

    editTodo(taskname, id) {
      console.log(taskname, id)
      this.todoToEdit = taskname;
      this.editingTask = id
    },

    swapThevalue(index) {
      if (this.swapValue1 === null) {
        this.swapValue1 = index;

        console.log(this.swapValue1);
      } else if (this.swapValue2 === null) {
        this.swapValue2 = index;
        console.log(this.swapValue2);
      }
      this.compare()
    },
    compare() {
      if (this.swapValue1 !== null && this.swapValue2 !== null) {
        const temp = this.todos[this.swapValue1].taskName;

        
        const updatedTodos = [...this.todos];
        updatedTodos[this.swapValue1] = {
          ...updatedTodos[this.swapValue1],
          taskName: this.todos[this.swapValue2].taskName,
          isSwap: false
        };
        updatedTodos[this.swapValue2] = {
          ...updatedTodos[this.swapValue2],
          taskName: temp,
          isSwap: false
        };

      
        this.todos = updatedTodos;

        this.swapValue1 = null;
        this.swapValue2 = null;
      }
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
  font-family: sans-serif;
  text-align: center;
  color: #2c3e50;
  padding: 60px 0;
  background-color: #0d0d0d;
  min-height: 100vh;
}

.todos_list_alert_text {
  font-size: 2rem;
  color: #CEBEA4;
}


@media only screen and (max-width: 500px) {
  html {
    font-size: 8px;
  }
}
</style>
