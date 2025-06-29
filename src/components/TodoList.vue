<template>
  <div class="app">
    <div class="todo-container">
      <h1>Todo List</h1>
      <div class="input-group">
        <input
          type="text"
          v-model="newTask"
          @keyup.enter="addTask"
          placeholder="Tambahkan tugas"
        />
        <button @click="addTask">Tambah</button>
      </div>

      <div class="filters">
        <button
          :class="{ active: filter === 'all' }"
          @click="filter = 'all'"
        >Show All</button>
        <button
          :class="{ active: filter === 'done' }"
          @click="filter = 'done'"
        >Done</button>
        <button
          :class="{ active: filter === 'undone' }"
          @click="filter = 'undone'"
        >Undone</button>
      </div>

      <p class="status">Tugas belum selesai: {{ incompleteCount }}</p>

      <transition-group name="list" tag="ul" class="todo-list">
        <li
          v-for="task in filteredTasks"
          :key="task.id"
          :class="{ completed: task.completed }"
        >
          <span @click="toggleTask(task)">
            {{ task.text }}
          </span>
          <button class="delete" @click="deleteTask(task.id)">✖</button>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [],
      filter: "all",
    };
  },
  computed: {
    incompleteCount() {
      return this.tasks.filter((task) => !task.completed).length;
    },
    filteredTasks() {
      if (this.filter === "all") return this.tasks;
      if (this.filter === "done") return this.tasks.filter((t) => t.completed);
      if (this.filter === "undone") return this.tasks.filter((t) => !t.completed);
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({
          id: Date.now(),
          text: this.newTask,
          completed: false,
        });
        this.newTask = "";
      }
    },
    toggleTask(task) {
      task.completed = !task.completed;
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((t) => t.id !== id);
    },
  },
};
</script>

<style scoped>
body {
  margin: 0;
  background: url('@/assets/background.jpg') no-repeat center center fixed;
  background-size: cover;
  font-family: 'Poppins', sans-serif;
}

.app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.todo-container {
  background: rgba(255, 255, 255, 0.85);
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  width: 400px;
  text-align: center;
}

h1 {
  margin-bottom: 1rem;
}

.input-group {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.input-group input {
  flex: 1;
  padding: 0.5rem;
  border-radius: 0.5rem;
  border: 1px solid #ccc;
  transition: border 0.3s;
}

.input-group input:focus {
  border-color: #007bff;
  outline: none;
}

.input-group button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 0 1rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background 0.3s;
}

.input-group button:hover {
  background-color: #0056b3;
}

.filters {
  margin-bottom: 1rem;
  display: flex;
  justify-content: center;
  gap: 0.5rem;
}

.filters button {
  background-color: #e0e0e0;
  border: none;
  padding: 0.3rem 0.8rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: background 0.3s;
}

.filters button.active {
  background-color: #007bff;
  color: #fff;
}

.filters button:hover {
  background-color: #007bff;
  color: #fff;
}

.status {
  margin-bottom: 1rem;
  color: #333;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-list li {
  background-color: #f9f9f9;
  border-radius: 0.5rem;
  margin-bottom: 0.5rem;
  padding: 0.5rem 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.3s, opacity 0.3s;
  cursor: pointer;
}

.todo-list li:hover {
  background-color: #e9e9e9;
}

.todo-list li.completed span {
  text-decoration: line-through;
  color: #aaa;
}

.delete {
  background: transparent;
  border: none;
  color: red;
  font-size: 1.2rem;
  cursor: pointer;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}
.list-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
