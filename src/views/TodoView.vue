<script setup>
import {onMounted, onUpdated, ref} from "vue";
import autoAnimate from "@formkit/auto-animate";

let id = 0;
let prevId = 0;
const newTodo = ref("");
let isUpdate = ref(false);
const list = ref();
const todos = ref([
  { id: id++, todo: "sleep" },
  { id: id++, todo: "take bath" },
  { id: id++, todo: "calling police" }
]);
const AddTodo = () => {
  if (isUpdate.value) {
    todos.value = todos.value.map((t) => {
      if (t.id === prevId) return { id: t.id, todo: newTodo.value };
      else return t;
    });
    newTodo.value = "";
    return;
  }
  todos.value.push({ id: id++, todo: newTodo.value });
  newTodo.value = "";
  isUpdate.value = false;
};
const UpdateTodo = (updateTodo) => {
  isUpdate.value = true;
  prevId = updateTodo.id;
  newTodo.value = updateTodo.todo;
};
const RemoveTodo = (id) => {
  todos.value = todos.value.filter((todo) => todo.id !== id);
};
onMounted(() => {
  autoAnimate(list.value);
});
onUpdated(() => {
  if (newTodo.value === "") {
    isUpdate.value = false;
  }
});
</script>

<template>
  <main class="container">
    <h1>Todo APP</h1>
    <form @submit.prevent="AddTodo" class="custom-form">
      <input v-model="newTodo" required placeholder="Add new todo..." autofocus>
      <button v-if="isUpdate">Update Todo</button>
      <button v-else>Add Todo</button>
    </form>
    <ul ref="list">
      <li v-for="todo in todos" :key="todo.id">
        <div style="width: 100%; padding-left: 3px">{{ todo.todo }}</div>
        <span>
          <button @click="UpdateTodo(todo)">Update</button>
          <button @click="RemoveTodo(todo.id)">X</button>
        </span>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
}

.custom-form {
  display: flex;
  gap: 10px;
  justify-content: space-between;
  border: #f2f2f2 2px solid;
  border-radius: 10px;
  padding: 10px;
}

.custom-form button {
  width: 30%;
}

input {
  font-size: 15px;
  border-radius: 10px;
  width: 100%;
  background-color: transparent;
  border: 0;
  color: #f2f2f2;
}

::placeholder{
  color: var(--color-text);
}

button {
  border-radius: 10px;
  padding: 10px;
  font-size: 15px;
  background-color: #f2f2f2;
}

ul {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding-top: 10px;
  list-style-type: none;
  margin-block: 0;
  margin-inline: 0;
  padding-inline: 0;
}

li {
  border: 2px #f2f2f2 solid;
  font-size: 15px;
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}

li span {
  width: 30%;
  display: flex;
  justify-content: space-between;
}

li span button {
  width: 100%;
}
</style>