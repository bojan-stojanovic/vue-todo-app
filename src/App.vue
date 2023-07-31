<template>
  <div>
    <div class="logos">
      <a href="https://vitejs.dev" target="_blank">
        <img src="/vite.svg" class="logo" alt="Vite logo" />
      </a>
      +
      <a href="https://vuejs.org/" target="_blank">
        <img src="./assets/vue.svg" class="logo" alt="Vue logo" />
      </a>
    </div>

    <div class="todo-app">
      <TodoInput @add-todo="onAddTodo"/>

      <div class="todo-app__list">
        <Transition name="fade" mode="out-in">
          <p v-if="todoList.length === 0">There are no todo's, yet...</p>

          <div v-else>
            <div class="list-filter">
              <p>Filters:</p>
              <div>
                <button data-filter="all" @click="onFilterList($event, 'all')">All</button>
                <button data-filter="done" @click="onFilterList($event, 'done')">Done</button>
                <button data-filter="open" @click="onFilterList($event, 'open')">Open</button>
              </div>
            </div>
            <div class="filter-data">
              <p>Active filter: {{ activeFilter }}</p>
              <p>Number of todo's: {{ totalNumberOfTodos }}</p>
            </div>

            <TodoCard v-for="(todo, index) in filteredToDoList" :key="index" :todo="todo" :index="index" @remove-card="onRemoveCard" @done="onDone" />
          </div>
        </Transition>
      </div>
    </div>

  </div>
</template>

<script setup>
import TodoInput from "./components/TodoInput.vue";
import TodoCard from "./components/TodoCard.vue";

import { computed, onMounted, ref } from "vue";

// DATA
const todoList = ref([]);
const filteredToDoList = ref([]);
const activeFilter = ref("all");

// COMPUTED
const totalNumberOfTodos = computed(() => {
  return filteredToDoList.value.length;
});

// METHODS
const onAddTodo = (todo) => {
  todoList.value = [todo, ...todoList.value];
  filteredToDoList.value = [...todoList.value];
}

const onRemoveCard = (cardIndex) => {
    // remove selected card from todoList
    todoList.value = [...todoList.value.filter((item, index) => index !== cardIndex)];
    filteredToDoList.value = [...todoList.value];
};

const onDone = (cardIndex) => {
    // toggle completed status for selected card
    filteredToDoList.value.at(cardIndex).completed = !filteredToDoList.value.at(cardIndex).completed;
};

const onFilterList = ({ target }, status) => {
  // get filter name
  const filterName = target.dataset.filter;

  // don't do anything if user clicked on already active filter
  if (filterName === activeFilter.value) return;

  // change active filter text
  activeFilter.value = filterName;

  // Filter list based on status
  if (status === "done") {
    filteredToDoList.value = [...todoList.value.filter(item => item.completed)];
  } else if (status === "open") {
    filteredToDoList.value = [...todoList.value.filter(item => !item.completed)];
  } else {
    filteredToDoList.value = [...todoList.value];
  }
};

const fetchData = () => {
  // fetch todo-list.json
  fetch("/data/todo-list.json")
    .then(response => response.json())
    .then(data => {
      // add json data to todoList
      todoList.value = data;
      filteredToDoList.value = [...todoList.value];
    })
    .catch(error => console.log(error));
};

onMounted(() => {
  fetchData();
});
</script>


<style scoped>
.logos {
  display: flex;
  justify-content: center;
  align-items: center;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.todo-app {
  max-width: 500px;
  margin: 0 auto;
}

.list-filter {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
  gap: .5rem;
}

.list-filter>div {
  display: flex;
  gap: .5rem;
}

.filter-data {
  text-align: left;
  margin-bottom: 1rem;
}
</style>
