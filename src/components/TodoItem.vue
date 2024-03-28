<template>
  <div class="todos_list">
    <div :class="todo.isDone ? 'done' : 'todo_title_list'">
      <div
        class="circle"
        :class="getPriorityClass(todo)"
        @click="doneTodo"
      ></div>
      <h3 class="todo_title_text">
        {{ todo.taskName }}
      </h3>
    </div>
    <div class="todo_logo">
      <span @click="swapTodo(index)" v-if="showMoveToHigh" class="icons">
        <i
          class="fa-solid fa-retweet"
          :class="todo.isSwap ? 'isclicked' : 'notclicked'"
        ></i>
      </span>
      <span @click="editTodo" v-if="showEdit" class="icons"
        ><i class="fa-regular fa-pen-to-square"></i
      ></span>
      <span @click="onDeleteTodo" v-if="showDelete" class="icons"
        ><i class="fa-regular fa-trash-can"></i
      ></span>
      <span
        v-if="showUpdateCurrentStatus && todo.currentStatus === 'created'"
        @click="updateCurrentStatus(todo.id)"
        class="icons"
      >
        <i class="fa-solid fa-angles-right"></i>
      </span>
      <span
        v-if="showUpdateCurrentStatus && todo.currentStatus === 'process'"
        @click="updateCurrentStatus(todo.id)"
        class="icons"
        ><i class="fa-solid fa-angles-left"></i
      ></span>
      <span  @click="subtaskHandler(todo)" v-if="showSubtask" class="icons subtask-btn"
        >Subtask</span
      >
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todo: Object,
    index: Number,
    showMoveToHigh: {
      type: Boolean,
      default: true,
    },
    showEdit: {
      type: Boolean,
      default: true,
    },
    showDelete: {
      type: Boolean,
      default: true,
    },
    showSubtask: {
      type: Boolean,
      default: true,
    },
    showUpdateCurrentStatus: {
      type: Boolean,
      default: true,
    },
  },
  inject: ["changeCurrentStatus"],
  data() {
    return {
      editedTodo: "",
    };
  },
  methods: {
    doneTodo() {
      this.$emit("done-todo", this.todo);
    },
    editTodo() {
      this.$emit("edit-todo", this.todo);
      this.$emit("edit-subTask", this.todo.taskName,this.todo.id);
    },
    onDeleteTodo() {
      this.$emit("delete-todo", this.todo.id);
      this.$emit("delete-subtask", this.index);
    },
    swapTodo(index) {
      this.$emit("move-to-high", this.todo, index);
    },
    updateCurrentStatus(id) {
      this.changeCurrentStatus(id);
    },
    subtaskHandler(todo) {
      this.$emit("subtask-handler", todo);
    },
    getPriorityClass(todo) {
      return {
        "high-priority": todo.selectedPriority === "high",
        "medium-priority": todo.selectedPriority === "medium",
        "low-priority": todo.selectedPriority === "low",
      };
    },
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
