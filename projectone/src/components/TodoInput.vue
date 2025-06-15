<template>
  <div class="todo-input">
    <input v-model="todoText" @keyup.enter="submit" placeholder="Enter a task" />
    <input type="date" v-model="dueDate" />
    <button @click="submit">➕</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todoText: '',
      dueDate: ''
    };
  },
  methods: {
    submit() {
      if (this.todoText.trim()) {
        this.$emit('add-todo', { text: this.todoText, due: this.dueDate });
        this.todoText = '';
        this.dueDate = '';
      }
    }
  }
};
</script>

<style scoped>
.todo-input {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}
input[type="text"], input[type="date"] {
  flex: 1;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
}
button {
  padding: 10px 15px;
  background: #42b983;
  border: none;
  color: white;
  border-radius: 8px;
  cursor: pointer;
}
</style>
