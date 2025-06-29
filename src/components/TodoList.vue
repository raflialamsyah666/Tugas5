<template>
  <div class="container">
    <h1>Todo List</h1>
    <form @submit.prevent="addTask" class="form">
      <input
        v-model="newTodo"
        placeholder="Tambahkan tugas"
      />
      <button type="submit">Tambah</button>
    </form>

    <transition-group name="fade" tag="ul" class="todo-list">
      <li v-for="todo in todoStore.todos" :key="todo.id" class="todo-item">
        <span
          :class="{ done: todo.done }"
          @click="todoStore.toggleDone(todo.id)"
          class="todo-text"
        >
          {{ todo.text }}
        </span>
        <button @click="todoStore.removeTodo(todo.id)" class="delete-btn">🗑️</button>
      </li>
    </transition-group>

    <p class="counter">Tugas belum selesai: {{ todoStore.incompleteCount }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useTodoStore } from '../stores/todo'

const todoStore = useTodoStore()
const newTodo = ref('')

function addTask() {
  if (newTodo.value.trim() !== '') {
    todoStore.addTodo(newTodo.value)
    newTodo.value = ''
  }
}
</script>

<style scoped>
/* container styling */
.container {
  max-width: 500px;
  margin: 40px auto;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

/* form styling */
.form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  transition: border 0.3s;
}
input:focus {
  outline: none;
  border-color: #007bff;
}
button {
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 10px 14px;
  cursor: pointer;
  transition: background-color 0.3s;
}
button:hover {
  background-color: #0056b3;
}

/* todo list styling */
.todo-list {
  list-style: none;
  padding: 0;
}
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #00f7ff6c;
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 10px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  transition: transform 0.2s;
}
.todo-item:hover {
  transform: scale(1.02);
}

.todo-text {
  cursor: pointer;
  transition: color 0.3s, text-decoration 0.3s;
}
.todo-text.done {
  text-decoration: line-through;
  color: #888;
}

/* delete button styling */
.delete-btn {
  background: transparent;
  border: none;
  font-size: 18px;
  cursor: pointer;
  transition: color 0.3s;
}
.delete-btn:hover {
  color: red;
}

.counter {
  margin-top: 10px;
  text-align: center;
  color: #666;
}

/* transitions */
.fade-enter-active, .fade-leave-active {
  transition: all 0.4s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
.fade-enter-to, .fade-leave-from {
  opacity: 1;
  transform: translateY(0);
}
</style>
