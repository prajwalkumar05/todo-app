<template>
    <div class="todos_list">
        <div :class="todo.isDone ? 'done' : 'todo_title_list'">
            <div @click="doneTodo(todo)" class="circle"></div>
            <h3 v-if="!todo.isEditing" class="todo_title_text">{{ todo.taskName }}</h3>
            <input v-else v-model="editedTodo" class="todo_input_edit">
        </div>


        <div class="todo_logo">
            <span @click="editTodo(todo)" class="icons"><i class="fa-regular fa-pen-to-square"></i></span>
            <span @click="onDeleteTodo(index)" class="icons"><i class="fa-regular fa-trash-can"></i></span>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoList',
    props: ["todo", "index"],
    data() {
        return {
            editedTodo: ''
        }
    },
    methods: {
        onDeleteTodo(todoID) {
            console.log(todoID)
            this.$emit('remove-todo', todoID);
        },

        doneTodo(todo) {
            todo.isDone = !todo.isDone
        },

        editTodo(todo) {
            todo.isEditing = !todo.isEditing

            if (todo.isEditing) {
                this.editedTodo = todo.taskName;
            } else {
                todo.taskName = this.editedTodo;
            }

        }
    }

}
</script>

<style>
.todos_list {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 1px solid #fff;
    padding: 1.5rem 2rem;
    border-radius: 10px;
    margin-bottom: 2rem;
    width: 30%;
    margin: 1rem auto;
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



@media only screen and (max-width: 500px) {
    html {
        font-size: 8px;
    }

    .todos_list {
        width: 60%;
    }
}
</style>