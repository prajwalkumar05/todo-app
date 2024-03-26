<template>
    <div class="list_con">
        <div class="todos_list">
            <div :class="todo.isDone ? 'done' : 'todo_title_list'">
                <div class="circle" :class="getPriorityClass(todo)" @click="doneTodo(todo)"></div>
                <h3 v-if="!todo.isEditing" class="todo_title_text">{{ todo.taskName }}</h3>
                <input v-else v-model="editedTodo" v-on:keyup.enter="editTodo(todo)" class="todo_input_edit">
            </div>

            <div class="todo_logo">
                <span @click="moveToHigh(todo, index)" class="icons"><i class="fa-solid fa-retweet"
                        :class="todo.isSwap ? 'isclicked' : 'notclicked'"></i></span>

                <span @click="editTodo(todo)" class="icons"><i class="fa-regular fa-pen-to-square"></i></span>
                <span @click="onDeleteTodo(todo.id)" class="icons"><i class="fa-regular fa-trash-can"></i></span>
                <span v-if="todo.currentStatus === 'created'" @click="updateCurrentStatus(todo.id)" class="icons"><i
                        class="fa-solid fa-angles-right"></i></span>
                <span v-else @click="updateCurrentStatus(todo.id)" class="icons"><i
                        class="fa-solid fa-angles-left"></i></span>
                <span @click="subtaskHandler" class="icons subtask-btn">Subtask</span>
            </div>
        </div>
        <div v-if="isSubTask === true" class="subtask-con">
            <SubTask :subTasks="todo.subTasks" @delete-subtask="deleteSubtask" @add-subtask="addSubtask" />
        </div>
    </div>
</template>

<script>
import SubTask from './SubTask.vue'

export default {
    name: 'TodoList',
    props: ["todo", "index"],
    emits: ["show-toast", "edit-todo", "swapthe-value", "remove-todo", "delete-subtask", "add-subtask"],
    inject: ['changeCurrentStatus'],
    components: {
        SubTask,
    },
    data() {
        return {
            editedTodo: '',
            isSubTask: false,
            newSubtask: '',
        }
    },
    methods: {
        onDeleteTodo(todoID) {
            this.$emit('remove-todo', todoID);
            this.$emit('show-toast', 'Todo Deleted');
        },

        doneTodo(todo) {
            todo.isDone = !todo.isDone

            // changing todo Process
            if (todo.isDone) {
                this.$emit('show-toast', 'Task Completed');
                todo.currentStatus = "done"
            } else {
                todo.currentStatus = "process"
            }
        },

        editTodo(todo) {
            this.$emit('edit-todo', { taskName: todo.taskName, id: todo.id }); // Emit event with the task name and id
        },

        getPriorityClass(todo) {
            // Return CSS class based on todo priority
            return {
                'high-priority': todo.selectedPriority === 'high',
                'medium-priority': todo.selectedPriority === 'medium',
                'low-priority': todo.selectedPriority === 'low'
            };
        },

        moveToHigh(todo, index) {
            todo.isSwap = !todo.isSwap
            // this.$emit('swapthe-value', index);
            this.$emit('swapthe-value', { index, todoStatus: todo.currentStatus });
        },

        updateCurrentStatus(id) {
            //using inject dirctly updating in parrent component
            this.changeCurrentStatus(id);
        },

        subtaskHandler() {
            this.isSubTask = !this.isSubTask
            console.log("i'm open")
        },

        addSubtask(id, subtask) {
            this.$emit('add-subtask', id, {
                updateId: this.editTodoId,
                todoId: this.todo.id,
                taskName: subtask.taskName,
                isDone: subtask.isDone,
                selectedPriority: subtask.selectedPriority
            });
        },

        deleteSubtask(subtaskIndex) {
            this.$emit('delete-subtask', {
                todoId: this.todo.id,
                subtaskIndex: subtaskIndex
            });
        }
    }
}
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
    border: 2px solid #FF5631;
    border-radius: 50%;

}

.todo_title_text {
    font-size: 2rem;
    color: #CEBEA4;
}

.todo_logo {
    display: flex;
    gap: 1.5rem;
    align-items: center;
}

.todo_logo .fa-regular {
    font-size: 2.2rem;
    color: #CEBEA4;
}

.fa-retweet {
    font-size: 2.2rem;
    color: #CEBEA4;
}

.fa-solid {
    font-size: 2.2rem;
    color: #CEBEA4;
}

.fa-angle-right {
    font-size: 2.2rem;
    color: #CEBEA4;
}

.notclicked {
    font-size: 2.2rem;
    color: #CEBEA4;
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
    border-bottom: 1px solid #CEBEA4;

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