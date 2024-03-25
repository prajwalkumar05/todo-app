<template>
    <div class="add_todo_container">
        <input v-model="inputValue" v-on:keyup.enter="handleSubmit" class="add_todo_input" type="text"
            placeholder="write your next task" />
        <select id="priority" v-model="selectedPriority">
            <option value="high">High</option>
            <option value="medium">Medium</option>
            <option value="low">Low</option>
        </select>
        <span class="btn" @click="handleSubmit"><i class="fa-solid fa-plus"></i></span>
    </div>
</template>

<script>
export default {
    name: 'TodoInput',
    props: ["initialValue"],
    emit: ["input-value"],
    data() {
        return {
            inputValue: "",
            selectedPriority: "medium"
        }
    },
    

    watch: {
        initialValue: {
            handler(newVal) {
                this.inputValue = newVal;
            },
        }
    },

    methods: {
        handleSubmit() {
            this.$emit('input-value', this.inputValue, this.selectedPriority);
            this.inputValue = ""
        },
    },
}
</script>

<style scoped>
.add_todo_container {
    height: 5rem;
    display: flex;
    width: 30%;
    margin: 3rem auto;
    align-items: center;
    /* border: 1px solid red; */

}

.add_todo_input {
    height: 100%;
    width: 100%;
    background-color: #1E1E1E;
    border-radius: 15px;
    outline: none;
    border: none;
    color: #CEBEA4;
    padding-left: 2.5rem;
    font-size: 1.6rem;
}

#priority {
    height: 100%;
    width: 40%;
    background-color: #1E1E1E;
    border-radius: 15px;
    outline: none;
    border: none;
    color: #CEBEA4;
    padding-left: 2.5rem;
    font-size: 1.6rem;
    margin-left: 0.5rem;
}

option {
    padding: 1rem;
    font-size: 1.6rem;
}

.add_todo_input::placeholder {
    font-size: 1.5rem;
    /* padding-left:1.5rem ; */
    color: #cebea45a;
}

.btn {
    padding: 1.6rem;
    border-radius: 50%;
    background-color: #FF5631;
    margin-left: 2rem;
}

.fa-plus {
    color: #0d0d0d;
    font-size: 1.6rem;
    font-weight: 800;
}

@media only screen and (max-width: 500px) {
    html {
        font-size: 8px;
    }

    .add_todo_container {
        height: 5rem;
        width: 60%;

    }
}
</style>