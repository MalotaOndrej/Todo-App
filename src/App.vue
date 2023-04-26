<template>
  <h1>To Do App</h1>
  <form @submit.prevent="addNewTodo">
    <label>Todo</label>
    <input
      v-model="newTodo"
      name="newTodo"
      placeholder="What?..."
      style="border-bottom: #1a1a1a 2px solid"
    />
    <button>ADD</button>
  </form>
  <ul v-for="(todo, index) in todos" :key="todo.id">
    <li class="todo">
      <input type="checkbox" v-model="todo.done" class="checkbox" />
      <input type="text" :class="{ done: todo.done }" v-model="todo.content" />
      <button class="delete" @click="deleteTodo(index)">delete</button>
    </li>
  </ul>
</template>

<script>
import { ref, onMounted, watch } from "vue";
export default {
  setup() {
    const newTodo = ref("");
    const todos = ref([]);

    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value,
      });
      newTodo.value = "";
    }

    watch(
      todos,
      (newVaL) => {
        localStorage.setItem("todos", JSON.stringify(newVaL));
      },
      { deep: true }
    );

    onMounted(() => {
      todos.value = JSON.parse(localStorage.getItem("todos") || []);
    });

    function deleteTodo(index) {
      todos.value.splice(index, 1);
    }
    return {
      onMounted,
      watch,
      todos,
      newTodo,
      addNewTodo,
      deleteTodo,
    };
  },
};
</script>

<style scoped>
input {
  margin: 0 5px;
  border: none;
  background-color: transparent;
}

.delete {
  background-color: brown;
}

.done {
  text-decoration: line-through;
}

.todo {
  margin: auto;
  height: auto;
  display: flex;
  gap: 5px;
  justify-content: space-between;
  align-items: center;
}

.checkbox {
  width: 1.3em;
  height: 1.3em;
  background-color: white;
  border-radius: 50%;
  vertical-align: middle;
  border: 1px solid #ddd;
  appearance: none;
  -webkit-appearance: none;
  outline: none;
  cursor: pointer;
}

.checkbox:checked {
  background-color: lightgreen;
}
</style>
