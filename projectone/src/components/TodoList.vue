<template>
  <ul class="todo-list">
    <TodoItem
      v-for="(todo, index) in todos"
      :key="index"
      :todo="todo"
      :index="index"
      @remove="removeTodo"
      @toggle-complete="toggleTodo"
    />
  </ul>
</template>

<script>
import TodoItem from './TodoItem.vue';
export default {
  props: ['todos'],
  components: { TodoItem },
  methods: {
    removeTodo(index) {
      this.$emit('remove-todo', index);
    },
    toggleTodo(index) {
      this.$emit('toggle-complete', index);
    }
  }
};
</script>

<style scoped>
.todo-list {
  list-style: none;
  padding: 0;
}
</style>
