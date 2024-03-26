<template>
    <div class="subtask">
        <h3 class="title">SubTask</h3>
        <p v-if="sortTasksByPriority === 0" class="subtask-title">No Subtask</p>
        <!-- <div class="subtask-box" v-for="(subtask, index) in sortTasksByPriority" :key="index">
            <div class="subtask-box-left" :class="subtask.isDone ? 'done' : 'todo_title_list'">
                <div @click="doneTodo(subtask)" class="circle" :class="getPriorityClass(subtask)"></div>

                <p  class="subtask-title">{{ subtask.taskName }}</p>
            </div>

            <div class="icon-grp">
                 <span @click="editSubTask(subtask.taskName,subtask.id)" class="icons"><i class="fa-regular fa-pen-to-square"></i></span>
            <span @click="deleteSubtask(index)" class="icons"><i class="fa-regular fa-trash-can"></i></span>
            </div>
            
        </div> -->
        <div class="add">
            <TodoList :todos="subTasks" @add-subtask="addSubtask" @delete-subtask="deleteSubtask" />
            <TodoInput @inputSub-value="addSubtask" />
        </div>
    </div>
</template>

<script>
import TodoInput from './TodoInput.vue';
import TodoList from './TodoList.vue';

export default {
    props: ['subTasks'],
    components:{
        TodoInput,
        TodoList
    },
    data() {
        return {
            newSubtask: '',
            priority: 'low',
            editTodoId:-1,
        };
    },
    computed: {
        sortTasksByPriority() {
            const priorityOrder = { high: 0, medium: 1, low: 2 };
            return this.subTasks.slice().sort((a, b) => {
                return priorityOrder[a.selectedPriority] - priorityOrder[b.selectedPriority];
            });
        },
    },
    methods: {
        addSubtask(inputvalue,selectedPriority) {

            console.log(inputvalue,selectedPriority)

            if (inputvalue.trim() !== '') {
                this.$emit('add-subtask',this.editTodoId,{taskName: inputvalue, isDone: false, selectedPriority: this.priority });
                this.newSubtask = '';
                this.editTodoId = -1 // Clear the input field after adding
            }
        },
        deleteSubtask(index) {
            this.$emit('delete-subtask', index);
        },
        editSubTask(editValue,id){
            this.newSubtask =editValue;
            this.editTodoId = id
            console.log("updateid"+this.editTodoId)
        },
        getPriorityClass(todo) {
            // Return CSS class based on todo priority
            return {
                'high-priority': todo.selectedPriority === 'high',
                'medium-priority': todo.selectedPriority === 'medium',
                'low-priority': todo.selectedPriority === 'low'
            };
        },
        doneTodo(todo){
            console.log(todo.isDone)
            todo.isDone= !todo.isDone
        }
    }
};
</script>


<style scoped>
.subtask {
    width: 100%;
    padding: 1rem;
}

.title {
    font-size: 18px;
    margin-bottom: 15px;
}

.subtask-box {
    display: flex;
    width: 90%;
    margin: 0 auto;
    border: 1px solid #fff;
    justify-content: space-between;
    padding: 0.8rem;
    margin-bottom: 0.5rem;
    border-radius: 5px;
    align-items: center;
}

.circle {
    height: 2.5rem;
    width: 2.5rem;
    border: 2px solid #FF5631;
    border-radius: 50%;

}

.subtask-box-left {
    display: flex;
    gap: 10px;
    align-items: center;
}

.subtask-title {
    font-size: 18px;
}

.done .subtask-title{
    font-size: 18px;
    text-decoration: line-through;
}

.todo_input_edit {
    margin-top: 25px;
    font-size: 18px;
    height: 35px;
    background-color: transparent;
    color: #fff;
    outline: none;
    border: 0;
    border-bottom: 2px solid #fff;
}

.priority-select {
    height: 35px;
    border-radius: 25px;
    background-color: transparent;
    color: #fff;
    background-color: #000;
    font-size: 16px;
    margin-left: 10px;
}
option{
    font-size: 14px;
}

.fa-regular {
    font-size: 20px;
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

.btn {
    padding: 1rem;
    border-radius: 50%;
    background-color: #FF5631;
    margin-left: 2rem;
    padding-top: 1.5rem;
}

.fa-plus {
    color: #0d0d0d;
    font-size: 1.6rem;
    font-weight: 800;
}

.done .circle {
    background-color: rgb(36, 223, 36);
    border: 2px solid rgb(36, 223, 36);
}

.done .todo_title_text {
    text-decoration: line-through;
}

.icon-grp{
    display: flex;
    gap: 10px;
}

.add{
    width: 100%;
}
</style>