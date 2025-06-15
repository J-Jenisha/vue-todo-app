<template>
  <li class="todo-item">
    <span @click="$emit('toggle-complete', index)" class="checkbox">
      {{ todo.completed ? '✅' : '⬜' }}
    </span>
    <span :class="{ completed: todo.completed }">
      {{ todo.text }}
      <small v-if="todo.due"> (Due: {{ todo.due }})</small>
    </span>
    <button @click="$emit('remove', index)">❌</button>
  </li>
</template>

<script>
export default {
  props: ['todo', 'index']
};
</script>

<style scoped>
.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #eee;
  margin-bottom: 10px;
  padding: 10px 15px;
  border-radius: 8px;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
.checkbox {
  cursor: pointer;
  margin-right: 10px;
}
button {
  background: none;
  border: none;
  color: red;
  font-size: 18px;
  cursor: pointer;
}
small {
  color: #888;
  margin-left: 5px;
}
</style>
