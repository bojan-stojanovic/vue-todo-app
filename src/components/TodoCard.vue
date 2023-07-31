<template>
    <div class="todo-app__card" :class="{ 'todo-app__card--done': todo.completed }">
        <p>{{ todo.message }}</p>
        <div class="buttons">
            <button class="done" @click="onDone">Done</button>
            <button class="remove" @click="onRemoveCard">Remove</button>
        </div>
    </div>
</template>

<script setup>
// EMITS
const emit = defineEmits(["removeCard", "done"]);

// PROPS
const props = defineProps(["todo", "index"]);

// METHODS
const onRemoveCard = () => {
    // Emit event to parent
    emit("removeCard", props.index);
};

const onDone = () => {
    // Emit event to parent
    emit("done", props.index);
};
</script>

<style scoped>
.todo-app__card {
    padding: 1rem;
    outline: 1px solid #fff;
    background-color: rgba(255, 255, 255, .2);
    border-radius: 1rem;
    text-align: left;
    margin-bottom: 1rem;
    display: grid;
    grid-template-columns: 5fr 1fr;
    gap: 1rem;
    transition: background-color .25s, outline-color .25s;
}

.todo-app__card p {
    grid-template-columns: 1 / 2;
}

.todo-app__card .buttons {
    grid-template-columns: 2 / -1;
    display: flex;
    flex-direction: column;
    gap: .5rem;
}

.todo-app__card .buttons .done:hover {
    border-color: rgb(0, 170, 0);
    color: rgb(0, 170, 0);
}

.todo-app__card .buttons .remove:hover {
    border-color: red;
    color: red;
}

.todo-app__card--done {
    background-color: rgba(0, 60, 0, .5);
    outline-color: rgb(0, 60, 0);
}

@media(prefers-color-scheme: light) {
    .todo-app__card {
        background-color: rgba(0, 0, 0, .2);
        color: #000;
    }

    .todo-app__card--done {
        background-color: rgba(0, 60, 0, .5);
        outline-color: rgb(0, 60, 0);
        color: #fff;
    }
}
</style>