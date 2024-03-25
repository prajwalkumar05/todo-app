<template>
    <div class="todo-process">
        <div class="title">
            <h1 class="title-text">{{ title }}</h1>
        </div>

        <div class="todolist-con">
            <draggable>
                <TodoList v-for="(todo, index) in todolist" :todo="todo" @remove-todo="deleteTodo" :key="index"
                    :index="index" @swapthe-value="swapThevalue" @edit-todo="editTodo" @add-subtask="addSubtask"
                    @delete-subtask="deleteSubtask" />
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
    emits: ['show-toast', 'edit-todo', 'swapthe-value', 'remove-todo', 'add-subtask', 'delete-subtask'], 
    data() {
        return {
            thisTodos: [],
        }
    },
    computed: {
        todolist() {
            return this.todos;
        }
    },
    
    methods: {
        deleteTodo(todoID) {
            this.$emit('remove-todo', todoID);
        },
        editTodo({ taskName, id }) {
            this.$emit('edit-todo', { taskName, id });
        },
        swapThevalue({ index, todoStatus }) {
            this.$emit('swapthe-value',  index, todoStatus );
        },
        addSubtask(id,subtask) {
            console.log("edit id"+id)
            this.$emit('add-subtask',id, subtask);
        },
        deleteSubtask({ todoId, subtaskIndex }) {
            console.log("its process"+todoId)
            this.$emit('delete-subtask', { todoId, subtaskIndex });
        },
    }
}
</script>


<style scoped>
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