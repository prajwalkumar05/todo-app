<template>
  <div class="list_con">
    <TodoItem
      :index="index"
      :key="todo.id"
      :todo="todo"
      @done-todo="doneTodo"
      @edit-todo="editTodo"
      @delete-todo="deleteTodo"
      @move-to-high="swapTodo"
      @update-current-status="updateCurrentStatus"
      @subtask-handler="subtaskHandler"
      :showMoveToHigh="true"
      :showEdit="true"
      :showDelete="true"
      :showUpdateCurrentStatus="true"
    />
    <div v-if="isSubTask === true" class="subtask-con">
      <SubTask
        :subTasks="todo.subTasks"
        :todo="todo"
        @delete-subtask="deleteSubtask"
        @add-subtask="addSubtask"
      />
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem.vue";
import SubTask from "./SubTask.vue";

export default {
  name: "TodoList",
  props: ["todos", "todo", "index"],
  data() {
    return {
      isSubTask: false,
      selectedTodo: null,
    };
  },
  watch: {
    "todo.subTasks": {
      deep: true,
      handler(newValue, oldValue) {
        console.log(newValue, oldValue);
        this.updateTodoCompletion(this.todo);
      },
    },
  },
  methods: {
    doneTodo(todo) {
      console.log("done")
      this.$emit("show-toast", "Task Completed");

      todo.isDone = !todo.isDone
      if(todo.isDone){
        todo.subTasks.forEach((subtodo) => (subtodo.isDone = true));
        todo.currentStatus = "done";

      }
      else{
        todo.subTasks.forEach((subtodo) => (subtodo.isDone = false));
        todo.currentStatus = "process";
      }

      

      // Check if any subtodos are pending
      // const hasPendingSubtodos = todo.subTasks.some(
      //   (subtodo) => !subtodo.isDone
      // );

      // If there are pending subtodos, prevent marking the main todo as done
      // if (hasPendingSubtodos) {
      //   console.log(
      //     "Cannot mark main todo as done while subtodos are pending."
      //   );

      //   this.$emit(
      //     "show-toast",
      //     "Cannot mark main todo as done while subtodos are pending."
      //   );
      //   return;
      // }

      // Update completion status of all subtodos
      
    },
    editTodo(todo) {
      // Emit event to edit todo
      this.$emit("edit-todo", todo);
    },
    deleteTodo(todoId) {
      console.log(todoId);
      // Emit event to delete todo
      this.$emit("remove-todo", todoId);
    },
    swapTodo(todo, index) {
      console.log("move to high");
      console.log(todo, index);
      // Emit event to move todo to high
      this.$emit("swapthe-value", this.index, todo.currentStatus);
    },
    updateCurrentStatus(todoId) {
      // Emit event to update current status
      console.log(todoId);
      this.$emit("update-current-status", todoId);
    },
    subtaskHandler(todo) {
      console.log("todo come from item");
      console.log(todo);
      this.isSubTask = !this.isSubTask;
      this.selectedTodo = todo;
    },
    addSubtask(editId, subtask) {
      console.log("todolist my name is" + subtask.taskName);
      // Emit event to add subtask
      this.$emit("add-subtask", editId, {
        todoId: this.selectedTodo.id,
        ...subtask,
      });
    },
    deleteSubtask(subtaskIndex) {
      // Emit event to delete subtask
      this.$emit("delete-subtask", {
        todoId: this.selectedTodo.id,
        subtaskIndex,
      });
    },
    updateTodoCompletion(todo) {
      const allSubtodosCompleted = todo.subTasks.every(
        (subtodo) => subtodo.isDone
      );

      console.log(allSubtodosCompleted);
      todo.isDone = allSubtodosCompleted;
      // Update main todo's completion status

      if (todo.isDone) {
        todo.currentStatus = "done";
      } else {
        if (todo.currentStatus === "created") {
          todo.currentStatus = "created";
        } else {
          todo.currentStatus = "process";
        }
      }
    },
  },
  components: {
    TodoItem,
    SubTask,
  },
};
</script>

<style scoped>
.list_con {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #fff;
  width: 90%;
  margin: 1rem auto;
  border-radius: 10px;
}

.todos_list {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #fff;
  padding: 1.5rem 2rem;
  border-radius: 10px;
  margin-bottom: 2rem;
  width: 100%;
  margin: 0;
}

.icons {
  cursor: pointer;
}

.todo_title_list {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.circle {
  height: 2.5rem;
  width: 2.5rem;
  border: 2px solid #ff5631;
  border-radius: 50%;
}

.todo_title_text {
  font-size: 2rem;
  color: #cebea4;
}

.todo_logo {
  display: flex;
  gap: 1.5rem;
  align-items: center;
}

.todo_logo .fa-regular {
  font-size: 2.2rem;
  color: #cebea4;
}

.fa-retweet {
  font-size: 2.2rem;
  color: #cebea4;
}

.fa-solid {
  font-size: 2.2rem;
  color: #cebea4;
}

.fa-angle-right {
  font-size: 2.2rem;
  color: #cebea4;
}

.notclicked {
  font-size: 2.2rem;
  color: #cebea4;
}

.isclicked {
  font-size: 2.2rem;
  color: red;
}

.done {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.input_div {
  height: 100%;
  width: 100%;
}

.todo_input_edit {
  width: 60%;
  height: 40px;
  background-color: transparent;
  color: #fff;
  font-size: 2rem;
  outline: none;
  border: 0;
  border-bottom: 1px solid #cebea4;
}

.done .circle {
  background-color: rgb(36, 223, 36);
  border: 2px solid rgb(36, 223, 36);
}

.done .todo_title_text {
  text-decoration: line-through;
}

.high-priority {
  border: 2px solid red;
}

.medium-priority {
  border: 2px solid green;
}

.low-priority {
  border: 2px solid yellow;
}

.subtask-con {
  width: 100%;
}

.subtask-btn {
  height: 30px;
  width: 60px;
  border: 1px solid #fff;
  border-radius: 25px;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media only screen and (max-width: 500px) {
  html {
    font-size: 8px;
  }

  .todos_list {
    width: 60%;
  }
}
</style>
