<template>
  <div id="app">
    <Navbar />
    <TodoCounter :todos-list="todos" />
    <PriorityIndicator />
    <div class="input-con">
      <TodoInput :initial-value="todoToEdit" @input-value="handleInputValue" />
    </div>

    <p v-if="todos.length === 0" class="todos_list_alert_text">Add some todos!</p>

    <MessageAlert :message="toastMessage" ref="toast" />

    <div class="process">
      <TodoProcess title="current" :todos="createdStatusTodos" key="index" @remove-todo="deleteTodo"
        @edit-todo="editTodo" @show-toast="showToast" @swapthe-value="swapThevalue" @add-subtask="addSubtask"
        @delete-subtask="deleteSubtask" />

      <TodoProcess title="process" :todos="processStatus" @remove-todo="deleteTodo" @edit-todo="editTodo"
        @show-toast="showToast" @swapthe-value="swapThevalue" @add-subtask="addSubtask"
        @delete-subtask="deleteSubtask" />

      <TodoProcess title="complete" :todos="completeStatus" @remove-todo="deleteTodo" @edit-todo="editTodo"
        @show-toast="showToast" @swapthe-value="swapThevalue" @add-subtask="addSubtask"
        @delete-subtask="deleteSubtask" />
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

export default {
  name: 'App',
  components: {
    Navbar,
    TodoCounter,
    TodoInput,
    MessageAlert,
    PriorityIndicator,
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
      subtaskupdateID: null
    }
  },
  provide() {
    return {
      changeCurrentStatus: this.changeCurrentStatus
    }
  },
  computed: {
    createdStatusTodos() {
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

      // if editing task is there it update that edit value else dirctly push new value
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
          currentStatus: "created",
          subTasks: []
        });
        this.showToast("Todo Added");
      }
    },

    deleteTodo(id) {
      console.log("todo"+id)
      this.todos = this.todos.filter((todo) => todo.id !== id);
      this.editingTask = null;
    },

    showToast(message) {
      this.toastMessage = message;
      this.$refs.toast.showToast();
    },

    editTodo(obj) {
      //add oldTodovalue to assign to todoToEdit for editting that value
      this.todoToEdit = obj.taskName;

      //using gobal variable for work easy assgin edit to id
      this.editingTask = obj.id;
    },

    swapThevalue(index, status) {
      if (this.swapValue1 === null) {
        this.swapValue1 = { index, status };
      } else if (this.swapValue2 === null) {
        this.swapValue2 = { index, status };
        this.compare();
      }
    },
    compare() {
      if (this.swapValue1 !== null && this.swapValue2 !== null) {

        //getting swap todo1value using function
        const todo1 = this.getTodoFromStatus(this.swapValue1.index, this.swapValue1.status);

        //getting swap todo2value using function
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
          //it returns index of createdStatus todos
          return this.createdStatusTodos[index];
        case 'process':
          return this.processStatus[index];
        case 'done':
          return this.completeStatus[index];
        default:
          return null;
      }
    },

    changeCurrentStatus(todoId) {
      console.log(todoId)
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
    },

    addSubtask(editid, subtask) {
      console.log("subtask final report")
      console.log("name is "+subtask.selectedPriority)
      // Set the ID of the subtask being updated
      console.log("editid id")
      console.log(editid)
      if(editid === -1){
        this.subtaskupdateID = null;
      }else{
        this.subtaskupdateID = editid;
      }
      
      

      // Find the current todo being edited or added
      const todo = this.todos.find(todo => todo.id === subtask.todoId);
      console.log(todo)

      if (todo) {
        // Check if a subtask is being updated
        if (this.subtaskupdateID !== null) {
          const updatedSubtasks = todo.subTasks.map(sub => {
            if (sub.id === this.subtaskupdateID) {
              return { ...sub, taskName: subtask.taskName };
            }
            return sub;
          });

          // Update the subtasks array
          todo.subTasks = updatedSubtasks;
          this.subtaskupdateID = null;
        } else {
          console.log("todo")
          // Add a new subtask if not updating an existing one
          todo.subTasks.push({
            id: new Date().getTime(),
            taskName: subtask.taskName,
            isDone: false,
            selectedPriority: subtask.selectedPriority,
          });
          
          this.showToast("Subtask Added");
        }
      } else {
        console.error('Todo item not found.');
      }
    },

    deleteSubtask({ todoId, subtaskIndex }) {
      console.log(todoId, subtaskIndex)
      // Find the todo by its ID
      const todoIndex = this.todos.findIndex(todo => todo.id === todoId);


      // If the todo is found
      if (todoIndex !== -1) {
        // Remove the subtask by its index
        this.todos[todoIndex].subTasks.splice(subtaskIndex, 1);
        this.showToast("Subtask Deleted");
      } else {
        console.error('Todo item not found.');
      }
    },
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

.input-con{
  width: 30%;
  margin: 0 auto;
}


@media only screen and (max-width: 500px) {
  html {
    font-size: 8px;
  }
}
</style>
