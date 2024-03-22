<template>
  <div id="app">
    <Navbar />
    <TodoCounter :todos-list="todos" />
    <PriorityIndicator />
    <TodoInput :initial-value="todoToEdit" @input-value="handleInputValue" />

    <p v-if="todos.length === 0" class="todos_list_alert_text">Add some todos!</p>

    <!-- <draggable>
      <TodoList v-for="(todo, index) in sortedTasks" :todo="todo" :key="index" @remove-todo="deleteTodo" :index="index"
        @edit-todo="editTodo" @show-toast="showToast" @swapthe-value="swapThevalue" />
    </draggable> -->


    <MessageAlert :message="toastMessage" ref="toast" />

    <div class="process">
      <TodoProcess title="current" :todos="sortedAndFilteredTasks" key="index" @remove-todo="deleteTodo"
        @edit-todo="editTodo" @show-toast="showToast" @swapthe-value="swapThevalue" />

      <TodoProcess title="process" :todos="processStatus" @remove-todo="deleteTodo" @edit-todo="editTodo"
        @show-toast="showToast" @swapthe-value="swapThevalue" />

      <TodoProcess title="complete" :todos="completeStatus" @remove-todo="deleteTodo" @edit-todo="editTodo"
        @show-toast="showToast" @swapthe-value="swapThevalue" />

    </div>

  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'

import TodoCounter from './components/TodoCounter.vue'
import TodoInput from './components/TodoInput.vue'
import MessageAlert from './components/MessageAlert.vue'
import PriorityIndicator from './components/PriorityIndicator.vue'
import TodoProcess from './components/TodoProcess.vue'
// import draggable from "vuedraggable";

export default {
  name: 'App',
  components: {
    Navbar,
    TodoCounter,
    TodoInput,
    MessageAlert,
    PriorityIndicator,
    // draggable,
    TodoProcess
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
    sortedAndFilteredTasks() {
      return this.sortTasksByPriority(
        this.todos.filter(todo => todo.currentStatus === 'created')
      );
    },

    processStatus() {
      return this.sortTasksByPriority(
        this.todos.filter(todo => todo.currentStatus === 'process')
      );
    },

    completeStatus() {
      return this.sortTasksByPriority(
        this.todos.filter(todo => todo.currentStatus === 'done')
      );
    }


  },

  provide() {

    return {
      changeCurrentStatus: this.changeCurrentStatus
    }

  },

  methods: {
    sortTasksByPriority(tasks) {
      const priorityOrder = { high: 0, medium: 1, low: 2 };
      return tasks.slice().sort((a, b) => {
        return priorityOrder[a.selectedPriority] - priorityOrder[b.selectedPriority];
      });
    },
    handleInputValue(inputValue, selectedPriority) {
      if (inputValue === "") {
        return;
      }

      if (this.editingTask !== null) {
        const updatedTodos = this.todos.map(todo => {
          if (todo.id === this.editingTask) {
            return { ...todo, taskName: inputValue };
          }
          return todo;
        });

        this.todos = updatedTodos;
        this.editingTask = null;
      } else {
        this.todos.push({
          id: this.counter++,
          taskName: inputValue,
          isDone: false,
          isEditing: false,
          selectedPriority: selectedPriority,
          isSwap: false,
          currentStatus: "created"
        });
        console.log(this.counter)
        this.showToast("Todo Added");
      }


    },

    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
      this.editingTask = null;
    },

    showToast(message) {
      this.toastMessage = message;
      this.$refs.toast.showToast();
    },

    editTodo(oldValue, id) {
      this.todoToEdit = oldValue;
      this.editingTask = id;
    },

    swapThevalue(index, status) {
    console.log("swap", index);
    if (this.swapValue1 === null) {
      this.swapValue1 = { index, status };
    } else if (this.swapValue2 === null) {
      this.swapValue2 = { index, status };
      this.compare();
    }
  },
  compare() {
    if (this.swapValue1 !== null && this.swapValue2 !== null) {
      const todo1 = this.getTodoFromStatus(this.swapValue1.index, this.swapValue1.status);
      const todo2 = this.getTodoFromStatus(this.swapValue2.index, this.swapValue2.status);

      // Swap the task names
      const temp = todo1.taskName;
      todo1.taskName = todo2.taskName;
      todo2.taskName = temp;

      todo1.isSwap = false;
      todo2.isSwap = false;

      // Reset 
      this.swapValue1 = null;
      this.swapValue2 = null;
    }
  },
  getTodoFromStatus(index, status) {
    switch (status) {
      case 'created':
        return this.sortedAndFilteredTasks[index];
      case 'process':
        return this.processStatus[index];
      case 'done':
        return this.completeStatus[index];
      default:
        return null;
    }
  },

    changeCurrentStatus(todoId) {
      const todoIndex = this.todos.findIndex(todo => todo.id === todoId);

      if (todoIndex !== -1) {
        const updatedTodo = { ...this.todos[todoIndex] }; 

        if (updatedTodo.currentStatus === 'created') {
          updatedTodo.currentStatus = 'process';
        } else if (updatedTodo.currentStatus === 'process') {
          updatedTodo.currentStatus = 'created';
        } else if (updatedTodo.currentStatus === 'done') {
          updatedTodo.currentStatus = 'process';
          updatedTodo.isDone = !updatedTodo.isDone; 
        }

        // Update the todo item in the todos array
        this.todos.splice(todoIndex, 1, updatedTodo);
      } else {
        console.error('Todo item not found.');
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

.process {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 60px;
}


@media only screen and (max-width: 500px) {
  html {
    font-size: 8px;
  }
}
</style>
