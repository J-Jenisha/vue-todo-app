<template>
  <div :class="['app-container', darkMode ? 'dark' : 'light']">
    <h1>📝 My Smart To-Do</h1>

    <button class="toggle-theme" @click="darkMode = !darkMode">
      {{ darkMode ? '🌞 Light Mode' : '🌙 Dark Mode' }}
    </button>

    <TodoInput @add-todo="addTodo" />

    <input
      type="text"
      v-model="searchQuery"
      class="search"
      placeholder="🔍 Search tasks..."
    />

    <div class="filters">
      <button :class="{ active: filter === 'all' }" @click="filter = 'all'">All</button>
      <button :class="{ active: filter === 'active' }" @click="filter = 'active'">Active</button>
      <button :class="{ active: filter === 'completed' }" @click="filter = 'completed'">Completed</button>
      <button @click="clearCompleted" class="clear-btn">🧹 Clear Completed</button>
    </div>

    <TodoList
      :todos="filteredTodos"
      @remove-todo="removeTodo"
      @toggle-complete="toggleComplete"
    />
  </div>
</template>

<script>
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';

export default {
  components: { TodoInput, TodoList },
  data() {
    return {
      todos: [],
      filter: 'all',
      searchQuery: '',
      darkMode: false
    };
  },
  computed: {
    filteredTodos() {
      return this.todos
        .filter(todo => {
          if (this.filter === 'active') return !todo.completed;
          if (this.filter === 'completed') return todo.completed;
          return true;
        })
        .filter(todo => todo.text.toLowerCase().includes(this.searchQuery.toLowerCase()));
    }
  },
  methods: {
    addTodo(newTask) {
      this.todos.push({
        text: newTask.text,
        completed: false,
        due: newTask.due || null
      });
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleComplete(index) {
      this.todos[index].completed = !this.todos[index].completed;
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    }
  },
  mounted() {
    const stored = localStorage.getItem("todos");
    const mode = localStorage.getItem("darkMode");
    if (stored) this.todos = JSON.parse(stored);
    this.darkMode = mode === 'true';
  },
  watch: {
    todos: {
      handler(val) {
        localStorage.setItem("todos", JSON.stringify(val));
      },
      deep: true
    },
    darkMode(val) {
      localStorage.setItem("darkMode", val);
    }
  }
};
</script>

<style scoped>
.app-container {
  max-width: 600px;
  margin: auto;
  padding: 30px;
  border-radius: 10px;
  transition: background 0.3s, color 0.3s;
}
.light {
  background: #f9f9f9;
  color: #222;
}
.dark {
  background: #1e1e1e;
  color: #fff;
}
h1 {
  text-align: center;
}
.toggle-theme {
  float: right;
  background: transparent;
  border: none;
  cursor: pointer;
  margin-bottom: 10px;
  font-size: 16px;
}
.search {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
}
.filters {
  text-align: center;
  margin-bottom: 20px;
}
.filters button {
  margin: 5px;
  padding: 6px 12px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}
.filters .active {
  background: #42b983;
  color: white;
}
.clear-btn {
  background: crimson;
  color: white;
}
</style>
