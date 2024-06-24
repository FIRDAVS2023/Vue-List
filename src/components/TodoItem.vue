<template>
  <div class="todo-item">
    <input type="checkbox" :checked="todo.completed" @click="toggleComplete" class="checkbox">
    <span :class="{ completed: todo.completed }" class="todo-text">{{ todo.text }}</span>
    <div class="actions">
      <button @click="editTodo" class="btn edit-btn">Изменить</button>
      <button @click="deleteTodo" class="btn delete-btn">Удалить</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['todo', 'index'],
  methods: {
    toggleComplete() {
      this.$emit('toggle-complete', this.index);
    },
    editTodo() {
      const newText = prompt('Изменить задачу', this.todo.text);
      if (newText) {
        this.$emit('edit-todo', this.index, newText);
      }
    },
    deleteTodo() {
      this.$emit('delete-todo', this.index);
    }
  }
}
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 0;
  border-bottom: 1px solid #ddd;
  transition: background-color 0.3s;
}

.todo-item:hover {
  background-color: #f9f9f9;
}

.checkbox {
  margin-right: 10px;
}

.todo-text {
  flex-grow: 1;
  padding: 5px;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.actions {
  display: flex;
  gap: 10px;
}

.btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.edit-btn {
  background-color: #4CAF50;
  color: white;
}

.edit-btn:hover {
  background-color: #45a049;
}

.delete-btn {
  background-color: #f44336;
  color: white;
}

.delete-btn:hover {
  background-color: #e53935;
}
</style>
