<template>
  <div id="app">
    <Navbar />
    <TodoCounter :todos-list="todos" />
    <PriorityIndicator />
    <TodoInput :initial-value="todoToEdit" @input-value="handleInputValue" />

    <p v-if="todos.length === 0" class="todos_list_alert_text">Add some todos!</p>

    <draggable>
      <TodoList v-for="(todo, index) in sortedTasks" :todo="todo" :key="index" @remove-todo="deleteTodo" :index="index"
        @edit-todo="editTodo" @show-toast="showToast" @swapthe-value="swapThevalue" />
    </draggable>




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
import draggable from "vuedraggable";

export default {
  name: 'App',
  components: {
    Navbar,
    TodoCounter,
    TodoList,
    TodoInput,
    MessageAlert,
    PriorityIndicator,
    draggable
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
      return this.todos.slice().sort((a, b) => priorityOrder[a.selectedPriority] - priorityOrder[b.selectedPriority]);
    }
  },

  methods: {
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
        });
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

    swapThevalue(index) {
      if (this.swapValue1 === null) {
        this.swapValue1 = index;
      } else if (this.swapValue2 === null) {
        this.swapValue2 = index;
        this.compare();
      }
    },
    compare() {
      if (this.swapValue1 !== null && this.swapValue2 !== null) {

        const todosCopy = [...this.sortedTasks];

        // Swap the task names
        const temp = todosCopy[this.swapValue1].taskName;
        todosCopy[this.swapValue1].taskName = todosCopy[this.swapValue2].taskName;
        todosCopy[this.swapValue2].taskName = temp;

        todosCopy[this.swapValue1].isSwap = false
        todosCopy[this.swapValue2].isSwap = false,

          this.todos = todosCopy;

        // Reset 
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
