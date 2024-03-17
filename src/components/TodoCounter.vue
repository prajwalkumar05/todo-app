<template>
    <div class="todo_done">
        <div class="todo_title">
            <h2 class="todo_primary_title">Todo Done</h2>
            <p class="todo_secondary_title">Keep it up</p>
        </div>
        <div class="total_complete">
            <h1 class="total_done_text">{{ todosList.length }}</h1>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TodoDone',
    props: ["todosList"],
    data() {
        return {
           
        }
    },
}
</script>

<style>
.todo_done {
    max-height: 100%;
    width: 30%;
    margin: 5rem auto;
    border: 1px solid #CEBEA4;
    border-radius: 2rem;
    color: #CEBEA4;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 5rem;
    padding: 3rem 0;
    flex-wrap: wrap;

}

.todo_primary_title {
    font-size: 2.5rem;
    font-weight: 800;

}

.todo_secondary_title {
    font-size: 2rem;

}

.total_complete {
    height: 150px;
    width: 150px;
    border-radius: 150%;
    background-color: #FF5631;
    display: flex;
    justify-content: center;
    align-items: center;

}

.total_complete .total_done_text {
    font-size: 2.8rem;
    color: #0d0d0d;
    font-weight: 800;
}

@media only screen and (max-width: 500px) {
    html {
        font-size: 8px;
    }

    .todo_done {
        width: 60%;
    }
}
</style>