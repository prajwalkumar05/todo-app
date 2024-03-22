<template>
    <div class="todo-process">
        <div class="title">
            <h1 class="title-text">{{ title }}</h1>

        </div>

        <div class="todolist-con">
            <draggable>
                <TodoList v-for="(todo, index) in todolist" :todo="todo" @remove-todo="deleteTodo" :key="index"
                    :index="index" @swapthe-value="swapThevalue" @edit-todo="editTodo" />
            </draggable>


        </div>

    </div>
</template>

<script>

import TodoList from './TodoList.vue'
import draggable from "vuedraggable";

export default {
    name: 'TodoProcess',
    components: {
        TodoList,
        draggable,
    },
    props: ["todos", "title"],
    emit: ["show-toast", "edit-todo", "swapthe-value", "remove-todo"],
    data() {
        return {
            thisTodos: [],
        }
    },
    computed: {
        todolist() {
            console.log(this.todos)
            return this.todos
        }
    },
    methods: {
        deleteTodo(todoID) {
            this.$emit('remove-todo', todoID);
        },
        editTodo(todo) {
            const taskName = todo.taskName;
            const id = todo.id;
            console.log(taskName,id)
            this.$emit('edit-todo', taskName, id); // Emit event with the task name and id
        },
        swapThevalue(todo) {
            const todoStatus = todo.todoStatus
            const index = todo.index;
            this.$emit('swapthe-value', index,todoStatus);
        },
    }
}
</script>

<style>
.todo-process {
    color: #fff;
    height: 350px;
    width: 30%;
    border: 1px solid #fff;
    overflow-y: scroll;
    scrollbar-width: none;
}

.title {
    border: 1px solid #fff;
    width: 30%;
    margin: 0 auto;
    padding: 8px 15px;
    border-radius: 25px;
    margin-top: 10px;
    background-color: transparent;
}

.title-text {
    font-size: 20px;
    color: #CEBEA4;

}

/* ::-webkit-scrollbar {
  width: 5px;
}

::-webkit-scrollbar-thumb {
  background: tomato; 
} */
</style>