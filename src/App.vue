<script setup lang="ts">
import { ref, reactive, watch } from 'vue';

interface Todo {
  id: number,
  text: string,
};

const todos = reactive<Todo[]>(JSON.parse(localStorage.getItem('todos-vue')!) || []);
const todoText = ref("");

watch(todos, (newTodos: Todo[]) => {
  localStorage.setItem('todos-vue', JSON.stringify(newTodos));
});

function addTodo() {
  if (todoText.value.trim() === "") {
    return;
  }
  const newTodo: Todo = {id: Date.now(), text: todoText.value};
  todos.push(newTodo);
  todoText.value = "";
  console.log("New Todo created.")
}

function removeTodo(todo: Todo) {
  const index = todos.indexOf(todo);
  if (index !== 1) {
    todos.splice(index, 1);
  }
  console.log(`Todo "${todo.text}" removed.`);
}
</script>

<template>
  <header>
    <h1>Todo List</h1>
  </header>
  <main>
    <div v-if="todos.length === 0">
      <h2>There are no todos.</h2>
      <p>Please create a new todo</p>
    </div>
    <div v-else>
      <ul v-for="todo in todos" :key="todo.id">
        <li>
          {{ todo.text }}
          <button @click="removeTodo(todo)">❌</button>
        </li>
      </ul>
    </div>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="todoText" placeholder="What do you need to do?" />
      <button type="submit">Add Todo</button>
    </form>
  </main>
  <footer>
    <p>Created by <a href="https://golaughlin.github.io/" target='_blank'>Gage O'Laughlin</a></p>
  </footer>
</template>

<style scoped>
</style>
