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
                        <button @click="subtaskHandler">Click</button>
            </div>
        </div>
        <div v-if="isSubTask === true">
            <p>Lava lonn</p>
            <input  v-model="editedTodo" v-on:keyup.enter="editTodo(todo)" class="todo_input_edit">
        </div>
    </div>

</template>

<script>
export default {
    name: 'TodoList',
    props: ["todo", "index"],
    emit: ["show-toast", "edit-todo", "swapthe-value,remove-todo"],
    inject: ['changeCurrentStatus'],
    data() {
        return {
            editedTodo: '',
            isSubTask:false,
        }
    },
    methods: {
        onDeleteTodo(todoID) {
            console.log(todoID)
            this.$emit('remove-todo', todoID);
            this.$emit('show-toast', 'Todo Deleted');
        },

        doneTodo(todo) {
            todo.isDone = !todo.isDone

            if (todo.isDone) {
                this.$emit('show-toast', 'Task Completed');
                todo.currentStatus = "done"
            }
            else {
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
            this.changeCurrentStatus(id);
        },

        subtaskHandler(){
            this.isSubTask= !this.isSubTask
        }

    }
}
</script>

<style>
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



@media only screen and (max-width: 500px) {
    html {
        font-size: 8px;
    }

    .todos_list {
        width: 60%;
    }
}
</style>