<template>
    <div class="todo-app__input">
        <input type="text" placeholder="Type todo..." v-model="todo" @input="onInputChange($event)"
            @keyup.enter="onAddTodo()">

        <Transition name="fade" mode="out-in">
            <span v-if="inputIsEmpty">Please enter something</span>
        </Transition>

        <button @click="onAddTodo()">Add</button>
    </div>
</template>

<script setup>
import { ref } from 'vue';

// EMITS
const emit = defineEmits(["addTodo"]);

// DATA
const todo = ref("");
const inputIsEmpty = ref(false);

// METHODS
const onInputChange = ({ target }) => {
    // check if there is something in input
    if (target.value.length) {
        // remove error message when user starts typing in input field
        inputIsEmpty.value = false;
    }
};

const onAddTodo = () => {
    // add todo card
    if (todo.value === "") {
        // check if input is empty and show error
        inputIsEmpty.value = true;
    } else {
        // create myTodo object if input is not empty
        const myTodo = {
            message: "",
            completed: false
        };

        // update message property with input value
        myTodo.message = todo.value;

        // Emit event to parent
        emit("addTodo", myTodo);

        // Clear input field
        todo.value = "";
    }
};
</script>

<style scoped>
.todo-app__input {
    display: grid;
    grid-template-rows: 1fr 1fr;
    gap: 1rem;
    margin-bottom: 2rem;
}

.todo-app__input input {
    grid-row: 1 / 2;
    grid-column: 1 / -1;
    padding: .5rem 1rem;
}

.todo-app__input input.test {
    color: red;
}

.todo-app__input span {
    grid-row: 2 / -1;
    grid-column: 1 / 2;
    color: red;
    text-align: left;
}

.todo-app__input button {
    grid-row: 2 / -1;
    grid-column: 2 / -1;
    justify-self: end;
}
</style>